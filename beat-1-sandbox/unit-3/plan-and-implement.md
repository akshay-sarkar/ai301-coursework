# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

akshay-sarkar

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5970805584

Plan for #72, following my repro above (commit `2f4e82f`).

**Cause:** `verify_password` in `core/security.py` returns `pwd_context.verify(...)` directly, and passlib raises `UnknownHashError` when it can't identify the stored hash. My control (a valid hash returns `True`) shows that hashing and the bcrypt backend work. The missing piece is handling passlib's "unrecognized hash" errors.

**Prior work:** I've seen PR #78, which takes the same approach (catch, return `False`, drop the `xfail`). @hskl18 also pushed a branch (`4e7ce33` on `fix/72-malformed-password-hash`) with the same `except ValueError` catch, plus the empty-string and truncated-hash cases. Per the house rules I'm building mine from my own repro, and I'll compare against both before opening my PR.

**Change:** wrap that call in `try/except ValueError` and return `False`. I checked that one clause covers both cases: `UnknownHashError` is a `ValueError` subclass, and the truncated hash @rahulkumargmu reported raises a plain `ValueError`:

```
$ python -c 'from passlib.exc import UnknownHashError, PasswordSizeError; print(issubclass(UnknownHashError, ValueError), issubclass(PasswordSizeError, ValueError))'
True True
$ python -c 'from core.security import verify_password; print(verify_password("password", "$2b$12$abc"))' 2>&1 | tail -1
ValueError: salt too small (bcrypt requires exactly 22 chars)
```

Per CONTRIBUTING, I'll also remove the `xfail` marker from `test_verify_with_wrong_hash_format`, and I'll add one test for the truncated-hash case.

**Not changing:** `hash_password`, the `CryptContext` config, the passlib/bcrypt versions (including the harmless `(trapped)` bcrypt-version log line), or the login route, which already treats `False` as invalid credentials.

**Known side effect** (first raised here by @kpoon72): as the first line of output above shows, `PasswordSizeError` is also a `ValueError`, so an oversized password returns `False` instead of raising. That matches how the login route already handles a failed check.

**Test:** my repro command should print `False` instead of raising, the control should still print `True`, `test_verify_with_wrong_hash_format` should pass without its marker, and `tests/unit/test_security.py` should pass in full.

---

## Your branch

**Branch**

fix/72-verify-password-unknown-hash

**Evidence**

**Before** (unit 2 repro, posted at https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5860889860, on `main` at `2f4e82f`):

```
$ python -c 'from core.security import verify_password; print(verify_password("password", "not_a_valid_bcrypt_hash"))'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File ".../core/security.py", line 37, in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
  File ".../passlib/context.py", line 2343, in verify
    record = self._get_or_identify_record(hash, scheme, category)
  File ".../passlib/context.py", line 2031, in _get_or_identify_record
    return self._identify_record(hash, category)
  File ".../passlib/context.py", line 1132, in identify_record
    raise exc.UnknownHashError("hash could not be identified")
passlib.exc.UnknownHashError: hash could not be identified

$ python -c 'from core.security import verify_password, hash_password; print(verify_password("password", hash_password("password")))'
(trapped) error reading bcrypt version
AttributeError: module 'bcrypt' has no attribute '__about__'
True

$ python -m pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format --runxfail -q -p no:cacheprovider
E           passlib.exc.UnknownHashError: hash could not be identified
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
1 failed, 24 deselected, 2 warnings in 0.47s

$ python -c 'from core.security import verify_password; print(verify_password("password", "$2b$12$abc"))' 2>&1 | tail -1
ValueError: salt too small (bcrypt requires exactly 22 chars)
```

**After** (branch `fix/72-verify-password-unknown-hash`, same venv: macOS 26.5, Python 3.11.5, passlib 1.7.4, bcrypt 4.3.0):

```
$ python -c 'from core.security import verify_password; print(verify_password("password", "not_a_valid_bcrypt_hash"))'
False

$ python -c 'from core.security import verify_password, hash_password; print(verify_password("password", hash_password("password")))' 2>&1 | tail -1
True

$ python -m pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -q -p no:cacheprovider 2>&1 | tail -2
-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
1 passed, 25 deselected, 2 warnings in 0.94s

$ python -m pytest tests/unit/test_security.py -q -p no:cacheprovider 2>&1 | tail -2
-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
26 passed, 2 warnings in 6.35s
```

The trigger now returns `False` instead of raising, the valid-hash control still returns `True`, and the covering test passes without its `xfail` marker. The full security file (26 tests, including the new `test_verify_with_malformed_bcrypt_hash` for `$2b$12$abc`) passes.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run, first draft of rubric.md + procedure.md + evidence-guide.md: `agreement: 20/20 scored items  (bar: 18/20: PASS)`. Categories: `clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.
2. Confirming full run after revising procedure.md (rubric.md and evidence-guide.md unchanged), saved with `--save-run eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`. Categories: `clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.

Between the runs I changed only procedure.md, after live plan-check runs on my own #72 plan exposed gaps the eval set didn't. Read-order step 3 now records open PRs and posted patches from any commenter, not only maintainers, and counts a pushed branch on a fork as a patch. Check-execution step 7 now fails a comment that ignores them. Step 1 now looks for policy files under `docs/` and `.github/` as well as the root. Evidence-gathering steps 1 and 5 say new output in a plan never overrides a repro control, and credits to thread members get checked. No `--only` re-runs were needed, since no package disagreed in either run.

**Package analysis**

pkg-16 (pandas, `read_csv(engine="pyarrow", dtype=str)` strips leading zeros). Gold label: `reject` (wrong-cause). My rubric decided `reject`; they agree.

The plan is long and confident. It blames the post-read cast: "The defect is in the pandas-side cast that runs after pyarrow returns its table", in `ArrowParserWrapper._finalize_pandas_output`, and proposes re-padding the strings. It looks buildable, with files named, one approach, and a bounded scope line. A rubric that only checked structure would accept it.

The check that holds it is `grounded-diagnosis`, because of step 4 of its own repro evidence. With inference on, "the table's column arrives as `int64` with value `1`; the zeros are already gone in the parsed table, before any cast to string could run." That control shows the symptom present before the blamed step runs. My pass condition fails a plan whose evidence shows "the symptom already present before the blamed step runs", and my procedure makes the grader list every CONTROL from the repro evidence before it reads the plan, so the plan's confidence doesn't set the frame. Step 3 points at the real cause too: pyarrow keeps the zeros when told to parse the column as string, so the fix belongs in what pandas asks pyarrow for, not in repairing the output afterwards. The re-padding approach also can't recover unknown widths. Its test plan ("the parser test suite passes") is weak as well, but the diagnosis check is what decides the reject.

**Check rationale**

`| grounded-diagnosis | The plan's stated cause (diagnosis / root-cause section), read against every step, control run, and artifact in the repro-evidence block (see evidence guide: Diagnosis and grounding) | Pass if the stated cause is consistent with every artifact in the repro evidence, including control runs and "works when X" observations. Fail if any control, step, or artifact shows the blamed component, layer, or step working correctly, or shows the symptom already present before the blamed step runs; or if the plan adopts a cause (its own, or a confident one from the thread) that the repro evidence rules out; or if the plan states no cause at all. | required |`

It reads this way because a wrong-cause plan usually looks like a good plan: files named, bounded, confident. A check phrased as "the diagnosis is supported by the evidence" invites the grader to accept a plausible story. I rejected that wording in favour of a test a stranger can apply mechanically: find each control in the repro evidence and ask whether it clears the thing the plan blames. The three fail shapes come from the three ways I saw evidence contradict a cause. A control shows the blamed component working (pkg-01's no-`-v` run, pkg-07's instance-method error still printing, pkg-11's top-level `collect`). The symptom exists before the blamed step (pkg-16's step 4). Or the plan adopts the thread's confident diagnosis while the evidence points elsewhere (calib-03's key bindings vs the timing matrix). "Including control runs" is explicit because controls are where these packages hide the deciding fact.

**Trade-offs**

`grounded-diagnosis` holds a plan to every artifact in the repro evidence, so it can reject a plan whose cause is right when the repro itself is noisy or incomplete. If a control run was mis-recorded, or a step captured an unrelated failure, a correct diagnosis looks "contradicted" and the plan is held. I accept that miss. The fix there is a better repro, and a plan built on evidence that contradicts it shouldn't go out anyway. It also says nothing about a cause the evidence is silent on: a plan blaming a component no step tested passes this check, and I rely on `executable` and `decisive-test` to catch whether that plan can be built and proven. Nothing changed elsewhere: I ran two full runs (before and after the procedure.md revision), and in both all 7 clear-accept packages came back accept, which means every one of them passed `grounded-diagnosis` (it's a required check), while all 4 wrong-cause packages came back reject. So on this set the check isn't over-rejecting honest plans.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
