# Contribution: Tests: init vitest tests to run in angular context || remove karma

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Akshay Sarkar
**GitHub Username:** akshay-sarkar
**Issue:** https://github.com/Babali42/DrumBeatRepo/issues/611
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built issue-scout (six authored gh queries plus ranked filters) and patched its strategy and rubric after the first live scan**

On 2026-10-22 I installed the scout over my Unit 1 issue-select skill. I rewrote my fit profile for all of GitHub (TypeScript/React/Node first, Python second) and wrote six `gh search issues` queries, each traced to a profile sentence, plus ranking rules (drop assigned, stale, discussion-labelled, or 10+-comment issues; rank established-project results and `bug` labels first, then fewest comments). Before the scan, a raw API check showed that "fewest comments first" surfaced brand-new personal repos, so I added an owner-scoped query for maintained TS/JS projects (vitest, eslint, mui, TanStack, storybook, …). The first scan then graded 5 and accepted 2, but all 3 rejects were years-old query-1 issues with fresh drive-by claims, and both accepts were false positives (mui-x#9493 was already fixed on master, and code-charity/youtube#4411's labels are stamped on by its bug template), so I bounded query 1 by date and age, and added a required `not_already_fixed` check and a template-label rule to `issue_wanted` in my rubric.

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched in two passes. My scout's first scan (six `gh search issues` queries from my fit profile, ranked by my filters) graded five issues and accepted two, but both were false positives: mui-x#9493 was already fixed on master, and code-charity/youtube#4411's labels are stamped on by its bug template. I patched the rubric for both. My first three picks from the Issue Selection site were all rejected: logixia#31 and nanocoder#1143 already had someone on them, and devtrack#164 was already fixed on main, caught by the `not_already_fixed` check I'd just added. Popular-repo good first issues kept losing the same way, so I changed tactics. I intersected the site's JS/TS list with a GitHub search for open issues with no assignee, no linked PR, and 0–1 comments. Eight of those candidates (meteor, three in backstage, hiero, es-git, npmx, swisspost) still had an open PR or a recent claim. Babali42/DrumBeatRepo#611 was the one that didn't. My scout graded it **accept** on all 7 required checks: opened by the owner on Oct 15, nobody on it, not fixed (`npm test` still runs Karma and nothing uses `TestBed`), and two acceptable solutions stated by the owner.

It fits my profile because it's TypeScript and test-runner tooling, the "developer tooling such as test runners" my profile names. It's Angular rather than React, which I want to learn. I'm taking the owner's first option (configure Vitest for Angular `TestBed` with an example service test) and not the full Karma removal, because the scout pointed out that option B means porting about 15 spec files and changing CI, which is bigger than "small and well bounded". On the tool itself: across three gradings the scout's verdicts held up when I checked them against the threads, and the two fixes from scan 1 caught real cases on later issues. But it still can't judge size. `scope_bounded` passed both options here, even though one is a migration. A size check for "either/or" issues is the next change I'd make.

**Claim comment:** https://github.com/Babali42/DrumBeatRepo/issues/611#issuecomment-6331477037

**Toolkit**

- Tool: issue-scout (my search strategy and rubric). I ran one scan, graded three site picks, then graded #611 directly. I found #611 by cross-checking the Issue Selection site against a GitHub search filtered to no assignee, no linked PR, and 0–1 comments.
- What it produced: scan 1 accepted 2 of 5 (both false positives); the three site picks were all rejected on one required check each; #611 was accepted on 7/7 required and 3/3 preferred checks.
- Difficulties: scan 1 couldn't see bugs already fixed on master or labels added by issue templates, and passed a one-commit practice repo (logixia). Popular good first issues were almost always claimed or had an open PR, which the ranking stage can't see.
- What changed: I added `not_already_fixed`, a template-label rule in `issue_wanted`, and a repo-history clause in `repo_in_use`; I bounded query 1 by date and issue age. Still to do: a size check in `scope_bounded` for issues that offer a small option and a migration option.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
