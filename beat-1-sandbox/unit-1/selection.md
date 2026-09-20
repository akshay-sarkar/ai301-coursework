# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72

**Verdict output**

The verdict must record `accept` for this issue. It does: the fenced JSON block below records `"verdict": "accept"` for issue #72, ranked first.

```
Ranked read-out (today 2026-09-20). All 3 accept. No required check failed. Repo checks identical across the three, since they share one repo.

  Repo-level (all three):
  - maintainer_alive pass: newest commit 2026-09-16 by human Aburke225.
  - repo_in_use pass: not archived, last push 2026-09-16, no release (not a failure).
  - ai_policy_ok pass: docs/CONTRIBUTING.md has no AI statement. No AI_POLICY.md or AGENTS.md. The PR template has no AI checkbox.

  Ranking, by fit profile:
  1. #72 verify_password raises UnknownHashError. Best fit. Fix is short: catch the exception in core/security.py, return False. Issue names the covering xfail test (tests/unit/test_security.py, H-05). Remove the
     marker to finish. That is the small, well-bounded, runnable-test target you want. Python gap barely matters.
  2. #62 health check uses missing redis_host. Also short: use settings.redis_url. It has reproduction steps. Weaker fit: repro needs Redis running. CONTRIBUTING.md says the pyproject.toml attr-defined suppression
     for api/routes/health.py is issue #62, so remove it too. Issue body doesn't name a test.
  3. #73 README vs .env.example mismatch. Smallest change, but docs-only. No test to run, so it gives less practice with the test and review workflow you want.

  Preferred checks: all three have good first issue, are opened by a COLLABORATOR, and name files. All three pass all preferred checks, so they don't separate the candidates. Ranking came from fit only.

  Notes:
  - House rule moot: no comments, no claims, no assignees on any of them.
  - No PRs exist in the repo at all, so none are linked or mentioned.
  - Per CONTRIBUTING.md, CI must be green on all five jobs, and the seeded-bug xfail(strict=True) marker must be removed in the fix PR.
  - Your first fork PR may need a maintainer to approve CI.

  [
    {
      "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
      "checks": [
        {"name": "maintainer_alive", "grade": "pass", "evidence": "Newest main commit 2026-09-16 (4 days ago) by human Aburke225; last 5 commits all human-authored"},
        {"name": "repo_in_use", "grade": "pass", "evidence": "isArchived=false; pushedAt 2026-09-16; latestRelease null (none published)"},
        {"name": "nobody_on_it", "grade": "pass", "evidence": "assignees []; 0 comments; 0 PRs in repo (search for #72 returns 0); timeline shows no cross-references"},
        {"name": "scope_bounded", "grade": "pass", "evidence": "Single bug in core/security.py with stated fix (return False on UnknownHashError); no comments, no debate; opened 2026-09-10"},
        {"name": "issue_wanted", "grade": "pass", "evidence": "Opened by Aburke225 (COLLABORATOR); label `good first issue`"},
        {"name": "ai_policy_ok", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no AI policy; no AI_POLICY.md or AGENTS.md; PR template has no AI checkbox"},
        {"name": "good_first_issue_label", "grade": "pass", "evidence": "Labels: bug, good first issue, api, tier-1"},
        {"name": "maintainer_endorsed", "grade": "pass", "evidence": "Opener Aburke225 is COLLABORATOR"},
        {"name": "actionable_detail", "grade": "pass", "evidence": "Names core/security.py and tests/unit/test_security.py; says to remove @pytest.mark.xfail (H-05)"}
      ],
      "verdict": "accept"
    },
    {
      "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62",
      "checks": [
        {"name": "maintainer_alive", "grade": "pass", "evidence": "Newest main commit 2026-09-16 (4 days ago) by human Aburke225; last 5 commits all human-authored"},
        {"name": "repo_in_use", "grade": "pass", "evidence": "isArchived=false; pushedAt 2026-09-16; latestRelease null (none published)"},
        {"name": "nobody_on_it", "grade": "pass", "evidence": "assignees []; 0 comments; 0 PRs in repo (search for #62 returns 0); timeline shows no cross-references"},
        {"name": "scope_bounded", "grade": "pass", "evidence": "Single bug in api/routes/health.py with stated cause and fix (use settings.redis_url instead of redis_host/redis_port); no debate; opened 2026-09-10"},
        {"name": "issue_wanted", "grade": "pass", "evidence": "Opened by Aburke225 (COLLABORATOR); label `good first issue`"},
        {"name": "ai_policy_ok", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no AI policy; no AI_POLICY.md or AGENTS.md; PR template has no AI checkbox"},
        {"name": "good_first_issue_label", "grade": "pass", "evidence": "Labels: bug, good first issue, api, tier-1"},
        {"name": "maintainer_endorsed", "grade": "pass", "evidence": "Opener Aburke225 is COLLABORATOR"},
        {"name": "actionable_detail", "grade": "pass", "evidence": "Names api/routes/health.py and core/config.py; gives steps to reproduce (GET /health, 503, redis_health_check_failed / AttributeError)"}
      ],
      "verdict": "accept"
    },
    {
      "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73",
      "checks": [
        {"name": "maintainer_alive", "grade": "pass", "evidence": "Newest main commit 2026-09-16 (4 days ago) by human Aburke225; last 5 commits all human-authored"},
        {"name": "repo_in_use", "grade": "pass", "evidence": "isArchived=false; pushedAt 2026-09-16; latestRelease null (none published)"},
        {"name": "nobody_on_it", "grade": "pass", "evidence": "assignees []; 0 comments; 0 PRs in repo (search for #73 returns 0); timeline shows no cross-references"},
        {"name": "scope_bounded", "grade": "pass", "evidence": "Single docs mismatch: make README.md and .env.example agree on OPENROUTER_API_KEY / LLM_PROVIDER; no debate; opened 2026-09-16"},
        {"name": "issue_wanted", "grade": "pass", "evidence": "Opened by Aburke225 (COLLABORATOR); label `good first issue`"},
        {"name": "ai_policy_ok", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no AI policy; no AI_POLICY.md or AGENTS.md; PR template has no AI checkbox"},
        {"name": "good_first_issue_label", "grade": "pass", "evidence": "Labels: bug, good first issue, docs, tier-1"},
        {"name": "maintainer_endorsed", "grade": "pass", "evidence": "Opener Aburke225 is COLLABORATOR"},
        {"name": "actionable_detail", "grade": "pass", "evidence": "Names README.md, .env.example and core/config.py; states the exact discrepancy"}
      ],
      "verdict": "accept"
    }
  ]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Smoke run, `--limit 2`, on the first rubric draft: `agreement: 2/2 scored items` (a partial run, not the bar run; earlier attempts before it failed on CLI login, not on the rubric).
2. First full run, first draft of the rubric (five required checks): `agreement: 16/20 scored items  (bar: 18/20: below the bar)`. Rows that disagreed: `issue-06  accept  reject  NO     failed: repo_in_use`, `issue-09  accept  reject  NO     failed: scope_bounded`, `issue-19  accept  reject  NO     failed: scope_bounded, good_first_issue_label (preferred), actionable_detail (preferred)`, `issue-20  reject  accept   NO     graded accept`. Category line: `categories: claimed 4/4  clear-accept 5/8  dead-repo 3/3  policy 1/1  scope 3/4`.
3. Partial re-run after revising the rubric, `--only issue-06,issue-09,issue-19,issue-20,issue-14,issue-16`: all six rows read `yes` (no agreement line is printed for partial runs). issue-14 and issue-16 were canaries: gold accept, I re-ran them because the new required check could have rejected them.
4. Final full run, saved with `--save-run eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`.

**Issue analysis**

issue-20 (excalidraw/excalidraw#11811, "Add company logo shape to the toolbar"). My first rubric decided `accept` (the run-1 row read `issue-20  reject  accept   NO     graded accept`, with no required check named as failed). The gold label is `reject`.

My rubric accepted it because every check I had was about the repo or about scope, and the issue passed all of them: excalidraw is active (`last push to any branch: 2026-08-04`), unassigned with no linked PRs, and the request is one bounded change ("Users should be able to place, resize, and move it like other elements"). None of my checks asked whether anyone wanted the change. The bundle header shows it: `opened by cursor[bot] (NONE) on 2026-08-02, state open, labels: none`, the thread has `(no comments)`, and the body ends with `Logo asset TBD.` It is an unlabeled feature request from a bot with no maintainer engagement, so a newcomer would be building something no maintainer asked for. I added the `issue_wanted` check to catch that, and after the change issue-20 grades reject.

**Check rationale**

The check I added after run 1, quoted from `tools/issue-select/rubric.md` as currently written:

`| issue_wanted | Issue header (opener username and association, labels) and Comments | ANY of: opened by a human with association OWNER, MEMBER, COLLABORATOR or CONTRIBUTOR; a maintainer comment agrees the change is wanted; label `good first issue` or `help wanted`. Fails when the opener is a bot account (username ends in `[bot]`) or has association NONE, AND there is no such label, AND no maintainer comment. | required |`

Reasoning behind its form: the evidence guide says a good-first-issue label is a claim of friendliness, and my repo and scope checks never asked whether the maintainers want the change. I made it an ANY of three signals (who opened it, a maintainer agreeing, or a label) so that one signal is enough, and only the case where all three are missing fails. I made it `required` because issue-20 passed every other required check, so a preferred check could not have changed its verdict.

**Trade-offs**

`issue_wanted` gives up good bug reports from outside contributors that have no label and no maintainer reply yet: a real bug opened by a newcomer with association NONE gets rejected until a maintainer touches it. I accept that miss for a first issue, since an untouched issue is one where a first PR is most likely to be ignored. I checked the cost with a canary: after adding it I re-ran issue-14 and issue-16 (both gold accept) with `--only`, and both stayed `accept`; issue-19 and issue-06 also flipped to `accept` in the same re-run. The full run then matched all 20.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Fit to my interests and the time available: Issue #72 is pretty straightforward: I just need to catch UnknownHashError in core/security.py and return False. The issue explicitly mentions tests/unit/test_security.py, so I’ll know it’s fixed once I remove the xfail tag and the test actually passes. Python isn't really my main stack—I mostly stick to React, TypeScript, and Node—but since the fix is super small, it seems like a solid, low-stress way to get familiar with the codebase. Plus, I want some more practice with the whole fork, branch, test, and PR review workflow. I'm expecting this to take around 2–3 hours total before Unit 2, which should give me plenty of time for the fix, running CI, and handling any PR feedback.

2. What the verdict identified correctly, and what I weighed that the rubric could not: The skill accepted #72 with every required check passing: a live repo (`Newest main commit 2026-09-16`), no assignee and no PRs, a single bounded bug, and the issue names the test to un-xfail (`tests/unit/test_security.py`). All three candidates passed every preferred check too, so the rubric could not separate them. The ranking came from my fit profile: a short fix with a runnable test suited me better than #73 (docs-only, no test) or #62 (reproducing it needs Redis running). [ADD: anything you weighed yourself, for example the language.]

3. Anticipated difficulty in claiming it: The biggest risk is just that another student might jump on this issue first since it’s tagged as a "good first issue." But according to Path Review's rules, shared claims are totally fine, so that shouldn't hold me back. The main bottleneck will probably be setup—getting my local Python environment up and running and making sure all five CI checks turn green. Since this is my first PR from a fork, a maintainer might also have to manually approve the CI workflow, which could slow down the initial review a bit. On the code side, I just need to make sure returning False on a bad hash doesn't break valid ones, so I’ll run the full test_security.py test suite instead of just the single test.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
