# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

akshay-sarkar

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5858447083

Hi, I'd like to work on this one as my first contribution to PathReview.

The issue is that `verify_password` in `core/security.py` raises `UnknownHashError` when the stored hash isn't a valid bcrypt hash, instead of returning `False`. The covering test is `test_verify_with_wrong_hash_format` in `tests/unit/test_security.py` (currently `xfail`, H-05).

Next I'll set up the project from `docs/SETUP.md` on my fork, run that test with the `xfail` marker removed, and post a repro comment here with my environment, the exact commands, and the output I get.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5860889860

Reproduced on current `main`: `verify_password` raises `passlib.exc.UnknownHashError` for a non-bcrypt hash instead of returning `False`.

**Environment**
- Code: my fork at `2f4e82f` (same as upstream `main`, no local changes)
- macOS 26.5, Python 3.11.5
- passlib 1.7.4, bcrypt 4.3.0
- Setup: a Python venv plus `pip install -e ".[dev]"`. I skipped the Docker services and `make setup` from `docs/SETUP.md`, since this path doesn't touch Postgres or Redis.

**Steps**
```
git clone https://github.com/codepath/pathreview-ai301-fa26-s3.git && cd pathreview-ai301-fa26-s3
git checkout 2f4e82f
python3.11 -m venv .venv && source .venv/bin/activate
pip install -e ".[dev]"
python -c 'from core.security import verify_password; print(verify_password("password", "not_a_valid_bcrypt_hash"))'
```

**Output** (the issue's trigger, same input as `test_verify_with_wrong_hash_format`):
```
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
```

**Covering test with the `xfail` marker ignored:**
```
$ python -m pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format --runxfail -q -p no:cacheprovider
>           raise exc.UnknownHashError("hash could not be identified")
E           passlib.exc.UnknownHashError: hash could not be identified

.venv/lib/python3.11/site-packages/passlib/context.py:1132: UnknownHashError
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
1 failed, 24 deselected, 2 warnings in 0.47s
```

**Control** (a valid bcrypt hash verifies normally):
```
$ python -c 'from core.security import verify_password, hash_password; print(verify_password("password", hash_password("password")))'
(trapped) error reading bcrypt version
AttributeError: module 'bcrypt' has no attribute '__about__'
True
```
The `(trapped)` message is passlib 1.7.4 logging that it can't read bcrypt 4.x's version attribute. passlib catches it itself, and the call still returns `True`, so I don't think it's related to this issue.

**Expected:** `verify_password("password", "not_a_valid_bcrypt_hash")` returns `False`, as the test asserts.
**Actual:** it raises `UnknownHashError` from `pwd_context.verify` (`core/security.py:37`).

Next I'll look at handling that exception inside `verify_password` and check that the rest of `tests/unit/test_security.py` still passes.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke run, `--limit 2`, first rubric draft: `agreement: 2/2 scored items` (partial run, not a bar run).
2. First full run, first draft (seven required checks, one preferred): `agreement: 17/20 scored items  (bar: 18/20: below the bar)`. Disagreements: `pkg-03  accept  reject   NO     failed: claims-backed, control-run`, `pkg-05  accept  reject   NO     failed: steps-rerunnable, control-run`, `pkg-10  accept  reject   NO     failed: claims-backed, control-run`. Categories: `clear-accept 5/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`.
3. Partial re-run after loosening `claims-backed` and `steps-rerunnable`, `--only pkg-03,pkg-05,pkg-10,pkg-13,pkg-15,pkg-17,pkg-18,pkg-20`: `agreement: 8/8 scored items` (partial; pkg-13, pkg-15, pkg-17, pkg-18, pkg-20 were canaries).
4. Confirming full run, saved with `--save-run eval-run.txt`: `agreement: 19/20 scored items  (bar: 18/20: PASS)`.

**Package analysis**

pkg-01 (httpie/cli#1640, missing `Content-Type: application/json` with one custom header). Gold label: `accept`. My rubric decided `accept` in runs 1 and 2, then `reject` in the saved run 4: `pkg-01  accept  reject   NO     failed: same-trigger`.

The issue's trigger is `https post pie.dev/post -v 'header1: xyz' x=1`. The report ran `http --offline post pie.dev/post 'header1: xyz' x=1` instead: `http` not `https`, and `--offline` in place of `-v`, so the request is printed without being sent. The report labels the change ("Steps (offline, prints the request without sending)") and the claim comment gives the reason ("no network needed with `--offline`"), but it never says in one place "I changed the command from the issue's, and here is why". My `same-trigger` check says "compared character-for-character with the trigger in the issue body" and passes only if "every difference is named in the report with a reason". Read strictly, the swap is visible but not named as a deviation in the report itself, so the grader failed it this time.

I did not edit `same-trigger` between runs 2 and 4, so this flip is grader variance on a borderline case, not my revision. The difference really is harmless: `--offline` prints the exact request the issue shows, and the pasted output matches the issue's headers line for line, with a control run that has the header. That is why gold says accept. The honest reading is that "character-for-character" plus "named in the report" is stricter than the check needs to be when the output itself proves the trigger fired.

**Check rationale**

`| claims-backed | The report's headline outcome (reproduced / cannot reproduce / confirmed / root cause) and any claim of testing on another version, machine, or release, in the repro report and the claim comment, each matched to a pasted artifact (see evidence guide: Honesty) | Pass if the headline outcome rests on a pasted artifact, and every other assertion is either shown, a precise observation stated alongside that artifact (a control run's result, a "behavior unchanged" note on the version the artifact came from), or hedged as a hypothesis ("suggests", "looks like", "likely"). An honest cannot-reproduce that shows its attempt and names what differed passes. Fail if the headline outcome has no pasted artifact behind it, a root cause is stated as verified with no transcript, results are claimed for a version/machine/release other than the one the artifact came from, or certainty words ("guaranteed", "confirmed on two machines") rest on nothing shown. | required |`

My first draft said "Pass if every assertion has a pasted artifact behind it". That rejected two gold accepts. pkg-03 pastes the failing `rg -nU ... -r '$1'` output but states its control run in words ("Dropping `-r '$1'` from the same command reports 1, 4, 7, 10 correctly"). pkg-10 is an honest cannot-reproduce that pastes the prompt but describes `starship explain` in words and hedges its theory ("which suggests my shell reports `PWD` differently"). Neither overclaims; they just don't paste every sentence. So I moved the bar to the headline outcome: that must rest on an artifact, while side observations tied to that artifact and hedged hypotheses pass. I kept the fail clauses for what actually breaks packages: a root cause stated as verified with nothing shown (pkg-15, "I verified this race condition"), certainty with no artifact (pkg-13, "guaranteed reproducible"), and results claimed for an environment that was never tested (pkg-17's generalization to the Store release).

**Trade-offs**

Loosening `claims-backed` and `steps-rerunnable` risked flipping rejects to accepts, so before the confirming run I re-ran the three misses plus five canaries with `--only pkg-03,pkg-05,pkg-10,pkg-13,pkg-15,pkg-17,pkg-18,pkg-20`: pkg-13 and pkg-15 (no-evidence, claims with nothing shown), pkg-17 (claims about an untested release), pkg-18 (steps that need a private monorepo, which the looser "described precisely enough to recreate" wording could have let through), and pkg-20 (the one disclosure package). All eight read `yes`, and the full run kept `no-evidence 4/4`, `unfollowable-comms 3/3`, and `disclosure 1/1`. What the looser `claims-backed` gives up: a report that pastes its main failure but narrates a false control run in words would now pass that check, since I no longer demand the control's output. I accept that because the headline artifact still has to show the issue's symptom (`behavior-shown`), and a lying control is rarer than an honest one stated briefly. The strict `same-trigger` wording costs me pkg-01 on some runs, as the package analysis explains; I kept it strict because it is the check written for silent input changes like pkg-02's prefix range in place of the issue's offset-from-end syntax and calib-03's `:` in place of `=`; loosening it to fix one borderline accept would weaken the wrong-target family.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
