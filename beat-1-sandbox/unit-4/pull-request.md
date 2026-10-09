# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/114

**Branch**

fix/72-verify-password-unknown-hash

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run, first draft (five required checks: plan-fidelity, description-matches-diff, decisive-evidence, clean-diff, standards-honored; one preferred: repo-checks-shown): `agreement: 17/20 scored items  (bar: 18/20: below the bar)`. Misses: `pkg-02  clear-accept    accept  reject   NO     failed: decisive-evidence, clean-diff`, `pkg-05  clear-accept    accept  reject   NO     failed: decisive-evidence`, `pkg-11  clear-accept    accept  reject   NO     failed: description-matches-diff`.
2. Partial, `--only pkg-02,pkg-05,pkg-11,pkg-07,pkg-14,pkg-18,pkg-12,pkg-17,pkg-09,calib-03,calib-04 --include-calibration`, after loosening clean-diff (unchanged lines re-printed inside an edited hunk are diff noise), description-matches-diff (judge substance, not wording), and decisive-evidence (not-broken items met by a test in the diff): `agreement: 7/9 scored items`. pkg-05 fixed, all canaries held, but pkg-02 and pkg-11 still failed decisive-evidence.
3. Partial, `--only pkg-02,pkg-11,pkg-05,pkg-04,pkg-10,pkg-07,pkg-14,calib-04 --include-calibration`, after saying a fix applied to sibling methods is covered by the issue's trigger, and that an after result stated against the same command counts when the before quotes the failure: `agreement: 7/7 scored items`.
4. Full run: `agreement: 17/20 scored items  (bar: 18/20: below the bar)`. New misses: `pkg-08`, `pkg-13`, `pkg-19`, all `clear-accept`, all `failed: decisive-evidence`. My run-2 rule required the plan's controls to be asserted by a test in the diff, and these three only stated their controls in words.
5. Partial, `--only pkg-08,pkg-13,pkg-19,pkg-02,pkg-11,pkg-07,pkg-14,pkg-04,calib-04 --include-calibration`, after removing that rule (controls never decide decisive-evidence): `agreement: 8/8 scored items`.
6. Confirming full run, saved with `--save-run eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`. Categories: `clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`.

**Package analysis**

pkg-13 (neovim, `vim.ui.open()` truncates a URL containing `&` on Windows). Gold label: `accept` (clear-accept, an honestly disclosed shortfall). My rubric decided `reject` in full run 4 (`failed: decisive-evidence`) and `accept` in runs 1 and 6.

The PR fixes the `&` case: it caret-escapes cmd metacharacters, and its evidence re-runs the issue's trigger `:lua =vim.ui.open("https://duckduckgo.com/?q=hello&t=brave"):wait()` with a quoted before (`{ code = 1, stderr = "'t' is not recognized ..." }`) and after (`{ code = 0, stderr = "" }`, browser shows both parameters). `%VAR%` expansion is deferred, and the deferral is recorded in the plan's note and restated in the description, so plan-fidelity and description-matches-diff pass on the honest-outcome rule.

Run 4 rejected it because of a rule I had added in run 2: test-plan items that check something that wasn't broken had to be "asserted by a test in the diff". pkg-13's plan names a control ("the no-ampersand control unchanged"), and its evidence only states "No-ampersand control unchanged." in words. So the grader marked the control "not covered" and failed the check, even though the failure mode itself had a decisive before/after. pkg-08 and pkg-19 failed the same way. That was the rubric's mistake, not the PR's: a control proves the fix didn't break the working path, but it says nothing about whether the bug is fixed, so it shouldn't gate the test-evidence check. In run 6, with that rule removed, my rubric accepts pkg-13: the one failure mode is "before and after shown", controls never decide the check, and the deferral is disclosed in both places.

**Check rationale**

`| decisive-evidence | The test evidence's before/after, read against the plan's test plan and the issue's reproduction trigger (see evidence guide: Test evidence) | Pass if, for every failure mode the issue's reproduction shows (each reported broken behavior and its trigger), the evidence re-runs that trigger (or a regression test built from it) and shows the failing output before and the fixed result after. The before must quote the actual failure (the error text, the wrong value, the wrong output). The after may be pasted output, or the observed result stated against the same command ("no error", "file not listed") when the before quotes the failure. When the plan applies one fix to sibling code paths the issue's repro doesn't exercise separately (the same change in three parallel methods), re-running the issue's trigger covers them. Test-plan items that check something that wasn't broken (a control, "a successful save still clears", a new warning or edge behavior) are not failure modes: a test in the diff, a stated result, or silence about them never fails this check. Fail if the evidence for a failure mode is a statement with no command and no quoted before ("tested locally", "works on my machine", "tests pass"); if it exercises only a control or an unchanged path (the case that never broke, a different method or code path than the issue's trigger); or if the issue or plan names several distinct failure modes (different triggers or different broken behaviors) and the evidence re-runs some and is silent on the rest. | required |`

This check took four revisions, and each sentence answers a package. My first draft required a before/after "for every failure mode the plan's test plan names", which read every test-plan line as a failure mode. pkg-02's "a successful save still clears" and pkg-05's "same-key redefine warns" became unrun failure modes. I then required those not-broken items to be asserted by a test in the diff, which fixed pkg-05 but broke pkg-08, pkg-13, and pkg-19, whose controls are stated in words. The final wording says controls never decide this check. pkg-02 also taught the sibling-paths sentence: one fix applied to `save_text`, `save_html`, and `save_svg` is covered by the issue's `save_text` trigger. pkg-11 taught the "stated against the same command" sentence: its after is the annotation `# after: searches normally, no error` under a before that quotes `dangling '\'`. I kept the three fail clauses unchanged throughout, because they are exactly the not-tested family: statement only (pkg-04's "colors work now", pkg-10's "verified working on my machine"), control or unchanged path only (pkg-07's single-file case, pkg-14's GET when the bug is in POST), and one of several distinct failure modes (calib-04).

**Trade-offs**

Loosening `decisive-evidence` twice risked flipping not-tested rejects, so each partial run carried canaries from that category. pkg-04 and pkg-10 (statement-only evidence), pkg-07 and pkg-14 (evidence on a control or unchanged path, the most direct risk once controls stopped deciding the check), and calib-04 (two distinct failure modes, only one re-run) stayed `reject` in runs 3 and 5, and the full run kept `not-tested 4/4`. What the final check gives up: a PR that fixes the reported case but breaks the working path, and only claims "control unchanged" in words, now passes this check. I accept that miss because the not-tested family is about whether the fix is proven, and a broken control would show up in the repo's own suite (my preferred `repo-checks-shown`) or in review. The "stated against the same command" allowance also gives up a little: an after that reads "no error" can't be verified the way pasted output can. I limited it to cases where the before quotes the actual failure and the command is shown, which still rejects pkg-10's "verified working on my machine for a full day".

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
