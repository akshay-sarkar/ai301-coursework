# Plan: issue #72, `verify_password` raises `UnknownHashError` instead of returning `False`

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72
My repro: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5860889860 (commit `2f4e82f`, macOS 26.5, Python 3.11.5, passlib 1.7.4, bcrypt 4.3.0)

## Diagnosis

`verify_password` in `core/security.py` returns `pwd_context.verify(...)` directly, and passlib's `CryptContext.verify` raises when it can't identify the stored hash instead of returning `False`. My repro shows the exception coming from that call, not from bcrypt:

```
  File ".../core/security.py", line 37, in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
  File ".../passlib/context.py", line 1132, in identify_record
    raise exc.UnknownHashError("hash could not be identified")
passlib.exc.UnknownHashError: hash could not be identified
```

The control in the same repro rules out a broken bcrypt backend: a valid hash still verifies.

```
$ python -c 'from core.security import verify_password, hash_password; print(verify_password("password", hash_password("password")))'
True
```

So the cause is that `verify_password` has no handling for passlib's "this isn't a hash I recognize" errors. Hashing and the bcrypt backend work.

## Prior work on the thread

PR #78 (opened 2026-09-27, "Fixes #72") takes the same approach: catch passlib's errors in `verify_password`, return `False`, and drop the `xfail`. @hskl18 also pushed a branch (`4e7ce33` on `fix/72-malformed-password-hash`, posted 2026-10-01) with the same `except ValueError` catch, plus the empty-string and truncated-hash cases. @kpoon72 raised the `PasswordSizeError` side effect on the thread. The house rules say open PRs and patches don't block a plan built from my own reproduction, so I'm building mine independently. I'll compare my diff against #78 and `4e7ce33` before opening my PR in Unit 4.

## Scope

In scope:
- Make `verify_password` return `False` when passlib can't identify or parse the stored hash.
- Remove the `@pytest.mark.xfail(strict=True, ...)` marker from `test_verify_with_wrong_hash_format`, as `docs/CONTRIBUTING.md` ("Working on a seeded bug: remove its xfail marker") requires. With `strict=True`, the test would otherwise fail CI once it passes.
- Add one regression test for a malformed bcrypt-prefixed hash (see Approach, step 3).

Not in scope:
- `hash_password`, the `CryptContext` configuration, or the bcrypt scheme.
- Upgrading or pinning passlib or bcrypt, including the `(trapped) error reading bcrypt version` message from passlib 1.7.4 with bcrypt 4.x. It's a separate, harmless log line, and my control shows verification still works.
- The login route in `api/routes/auth.py`. It already treats `False` as "invalid credentials" (`if not user or not verify_password(...)`), so it needs no change.
- Logging or alerting on malformed stored hashes.

## Files

- `core/security.py`: `verify_password`
- `tests/unit/test_security.py`: `test_verify_with_wrong_hash_format` (remove the marker), plus one new test in the same class

## Approach

1. Create branch `fix/72-verify-password-unknown-hash` from `main` on my fork.
2. In `verify_password`, wrap the `pwd_context.verify(...)` call in `try/except ValueError` and `return False` in the except branch. `passlib.exc.UnknownHashError` is a subclass of `ValueError`, so one clause covers both the unrecognized hash and the malformed-bcrypt case. I'm not using a bare `except`, so unrelated errors still surface.
3. Add `test_verify_with_malformed_bcrypt_hash`, asserting that `verify_password("password", "$2b$12$abc")` returns `False`. Another repro on this thread (rahulkumargmu) reported that this truncated hash raises a plain `ValueError` rather than `UnknownHashError`, and I've confirmed it (see Verified before building).
4. Remove the `xfail` marker from `test_verify_with_wrong_hash_format`.
5. Run the repro and tests below, then `make check`.
6. Commits use Conventional Commits, e.g. `fix(security): return False for unrecognized password hashes`.

## Test plan

1. Re-run my repro trigger. Before the fix (posted output), it raises `passlib.exc.UnknownHashError: hash could not be identified`. **After the fix it should print `False`.**
   ```
   python -c 'from core.security import verify_password; print(verify_password("password", "not_a_valid_bcrypt_hash"))'
   ```
2. Re-run my control. **It should still print `True`.** This shows valid hashes are unaffected.
   ```
   python -c 'from core.security import verify_password, hash_password; print(verify_password("password", hash_password("password")))'
   ```
3. Run the covering test with its marker removed. Before the fix it fails (`1 failed`). **After the fix it should report `1 passed`.**
   ```
   python -m pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -q
   ```
4. **The new malformed-hash test should pass, and the full security test file should pass with no failures.**
   ```
   python -m pytest tests/unit/test_security.py -q
   ```

## Verified before building

Both exception types the catch relies on are `ValueError` subclasses, and the truncated hash raises a plain `ValueError`:

```
$ python -c 'from passlib.exc import UnknownHashError, PasswordSizeError; print(issubclass(UnknownHashError, ValueError), issubclass(PasswordSizeError, ValueError))'
True True
$ python -c 'from core.security import verify_password; print(verify_password("password", "$2b$12$abc"))' 2>&1 | tail -1
ValueError: salt too small (bcrypt requires exactly 22 chars)
```

So `except ValueError` covers both the issue's case and the truncated-hash case (first reported on the thread by rahulkumargmu). It also catches `PasswordSizeError` (first raised on the thread by kpoon72), so an oversized password returns `False` instead of raising. I accept that: the login route already treats `False` as "invalid credentials", which is the right answer for a password passlib refuses to check.

## Risks and unknowns

- A `None` stored hash (a user row with no password) may raise `TypeError`. It isn't part of this issue, and I'm leaving it alone unless a test shows the login route can hit it.
- I haven't read PR #78's tests. If it already adds the truncated-hash test, I'll note that when I compare diffs before Unit 4.

## Deviations

The code change matches the plan. `verify_password` wraps `pwd_context.verify` in `try/except ValueError` and returns `False`, the `xfail` marker is gone from `test_verify_with_wrong_hash_format`, and `test_verify_with_malformed_bcrypt_hash` is added right after it. No other files changed. The diff is 2 files: `core/security.py` (+6/-1) and `tests/unit/test_security.py` (+4/-4).

One difference from Approach step 5: I ran the test plan and the full `tests/unit/test_security.py` file (26 passed) but didn't run the repo-wide `make check`. The change touches only `verify_password`, and its one caller (`api/routes/auth.py`) already treats `False` as invalid credentials, so I relied on the security test file and the repro for this unit. I'll run `make check` before opening the PR in Unit 4.

The posted plan is still accurate, so I didn't add a follow-up comment on the issue.
