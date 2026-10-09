---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer one question about one PR package: **is this ready to submit?** A PR package is a candidate pull request (its title, its description, its commits, its diff, and its test evidence), read against the plan it claims to implement (including that plan's deviation notes) and the issue the plan belongs to, under the repo's stated standards (PR template, contributing guide, AI policy). Do not grade the plan, the code style in general, or whether the issue is worth fixing. Grade only whether this PR, as it would be opened, is faithful to its plan, proven, reviewable, and compliant with what the repo asks.

## Inputs and modes

Decide the mode first. If you were given an eval package bundle (a markdown file with `## Repo facts`, `## Issue`, `## Thread highlights`, `## Plan context`, and `## Candidate PR`), you are in eval mode. Otherwise you are in live mode.

**Live mode** reads exactly these inputs:

1. `plan.md` in the student's working directory, including its `## Deviations` section. This is the plan the PR must match.
2. The branch's diff: run `git diff main...HEAD` (three dots) from the working copy. This is every committed change the branch makes relative to `main`. Also run `git diff main...HEAD --stat` and `git log main..HEAD --oneline` for the file list and commits. Uncommitted changes are not part of the PR; ignore them.
3. The draft PR title and description in `pr_draft.md` (the title is the first line).
4. The test evidence in `test_evidence.md`.
5. The issue: the URL the student gives. Read its thread (via `gh`, the GitHub API, or the web), the repo's `.github/PULL_REQUEST_TEMPLATE.md`, and `docs/CONTRIBUTING.md` (plus any AI_POLICY.md or AGENTS.md at the root, in `docs/`, or in `.github/`).

A house-chain student reads the house plan and the house repro pack instead of their own plan and repro; the same checks grade the same things. If a named file is missing, the evidence it would supply is absent, and the checks that need it grade on that absence. Do not read other files in the working directory as substitutes.

**Eval mode**: the bundle is the whole world. Every fact comes from the bundle text. Fetch nothing, run nothing, read no other file. Eval mode always grades a complete package: every check in the rubric, the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. If its `Repo:` line still holds a bracketed placeholder, stop without grading and tell the student to fill the `Repo:` line in `scope.md` with their section's Path Review repo. Refuse to grade a PR whose issue is not in the scoped repo, and say why. Apply the house rules scope.md lists when reading evidence (for example, a classmate's PR on the same issue does not block this one). Never guess a scope. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` after grading the checks. Hold the outgoing text, the PR title and description in `pr_draft.md`, against each rule. In the summary, list every rule the draft breaks, quoting the rule and the offending line. The voice guide never changes the verdict on its own, because voice is the student's personal standard and the rubric owns the verdict; only a rubric check that reads the voice guide could change it. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read the components in this order, every run:

1. `rubric.md`: the checks (name, evidence, pass condition, weight) and the verdict rule. The rubric decides what passes.
2. `references/evidence-guide.md`: where each evidence family lives, in a bundle and live, and what good looks like there.
3. `procedure.md`: the operating steps. Execute them as written, in order.

When the procedure is silent on a step you need, do not improvise a step. Make the most literal reading of the rubric's pass condition, grade, and report the procedure gap in your summary under "Procedure gaps", so the student can fix it.

If `rubric.md` has no filled check rows or no verdict rule, or `procedure.md` has no filled steps, refuse to grade: say which file is empty and stop, without emitting a verdict. A tool that invents checks at runtime produces noise that looks like judgment.

## Verdict and output

The verdict is binary: `accept` means ready to submit, `reject` means hold. There is no third verdict and no score; reservations go in check evidence lines.

Before the JSON block, print a short readable summary: one line per check with its grade and the deciding fact, then (live mode) voice-guide notes, then any procedure gaps. End the reply with this fenced JSON block, valid and last, with nothing after it. List every rubric check, in rubric order.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

In live mode, `item` is the issue URL (the PR does not exist yet). In eval mode, it is the bundle id.

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse, complete PR can be ready, and a beautiful, confident one can hide drift. Read the diff itself against the plan, the evidence itself against the test plan, and the description itself against the diff. Never let formatting or confidence decide a check.
- **The rubric decides.** If a check passes by its stated condition but feels wrong, it passes. Note the tension in the summary; the fix belongs in the rubric.
- **The procedure decides how.** Follow `procedure.md` as written, and report its gaps instead of silently filling them.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's verdict rule directs. Where the rule is silent, an unverifiable claim is a failing one: a PR you cannot verify from the package is not ready to submit.
- **Honest shortfalls are not failures.** A PR that does less than everything, and says so in the plan's deviation notes and the description, is judged as scoped. Only an unannounced gap is drift.
