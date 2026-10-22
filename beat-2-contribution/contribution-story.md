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

**Unit 6: Installed the voice guard with an Upstream section in my voice guide, and wrote a repo memory file for DrumBeatRepo**

I added an Upstream section to my voice guide (claims, questions, pings, each with a wrong/right pair) and wired `check_voice.py` as a PreToolUse hook, which stopped `gh issue comment --help` in the wiring test as intended. What fell short: I had already posted my #611 claim through the GitHub website, so the guard never saw it; I ran `check_voice.py --draft` on the posted text afterwards (it passed) and now post through Claude Code. My `CLAUDE.local.md` for DrumBeatRepo records what took me a while to find: there's no CONTRIBUTING.md, so CI's `angular-test` action defines "all tests"; a `test-vitest` script already exists; Karma needs a Scala-built `engine/main.js`; and Angular 22 rejects my default Node 25. After the owner replied I updated it to say: reuse `test-vitest` and leave `npm test` alone.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched in two passes. My scout's first scan (six `gh search issues` queries from my fit profile, ranked by my filters) graded five issues and accepted two, but both were false positives: mui-x#9493 was already fixed on master, and code-charity/youtube#4411's labels are stamped on by its bug template. I patched the rubric for both. My first three picks from the Issue Selection site were all rejected: logixia#31 and nanocoder#1143 already had someone on them, and devtrack#164 was already fixed on main, caught by the `not_already_fixed` check I'd just added. Popular-repo good first issues kept losing the same way, so I changed tactics. I intersected the site's JS/TS list with a GitHub search for open issues with no assignee, no linked PR, and 0–1 comments. Eight of those candidates (meteor, three in backstage, hiero, es-git, npmx, swisspost) still had an open PR or a recent claim. Babali42/DrumBeatRepo#611 was the one that didn't. My scout graded it **accept** on all 7 required checks: opened by the owner on Oct 15, nobody on it, not fixed (`npm test` still runs Karma and nothing uses `TestBed`), and two acceptable solutions stated by the owner.

It fits my profile because it's TypeScript and test-runner tooling, the "developer tooling such as test runners" my profile names. It's Angular rather than React, which I want to learn. I'm taking the owner's first option (configure Vitest for Angular `TestBed` with an example service test) and not the full Karma removal, because the scout pointed out that option B means porting about 15 spec files and changing CI, which is bigger than "small and well bounded". On the tool itself: across three gradings the scout's verdicts held up when I checked them against the threads, and the two fixes from scan 1 caught real cases on later issues. But it couldn't judge size: `scope_bounded` passed both options, even though one is a migration.

In Unit 6 I replaced `scope_bounded` with the course's `scope-feasible` check and added a difficulty note, then regraded #611. The difficulty note came back **ambitious**: Angular `TestBed` under Vitest likely needs a setup file and a DOM environment, and the `npm test` question was open. That's the size signal I was missing. The regrade also exposed a new bug. It rejected #611 on `nobody_on_it` because of my own claim comment, since the rubric had no exception for the person running the scout. With my comment ignored, every required check passed, so I patched `nobody_on_it` to skip the `gh` user's own claims. I asked the open question in my claim, and the owner answered the same day: reuse the existing `test-vitest` script, leave `npm test` on Karma, and focus on getting `TestBed` working with a first example.

**Claim comment:** https://github.com/Babali42/DrumBeatRepo/issues/611#issuecomment-6331477037

**Toolkit**

- Tool: issue-scout (my search strategy and rubric). I ran one scan, graded three site picks, then graded #611 directly. I found #611 by cross-checking the Issue Selection site against a GitHub search filtered to no assignee, no linked PR, and 0–1 comments.
- What it produced: scan 1 accepted 2 of 5 (both false positives); the three site picks were all rejected on one required check each; #611 was accepted on 7/7 required and 3/3 preferred checks.
- Difficulties: scan 1 couldn't see bugs already fixed on master or labels added by issue templates, and passed a one-commit practice repo (logixia). Popular good first issues were almost always claimed or had an open PR, which the ranking stage can't see.
- What changed: I added `not_already_fixed`, a template-label rule in `issue_wanted`, and a repo-history clause in `repo_in_use`; I bounded query 1 by date and issue age. In Unit 6 I replaced `scope_bounded` with `scope-feasible` plus a difficulty note (which flagged #611 as ambitious), and patched `nobody_on_it` after it rejected #611 for my own claim.

---

## Understanding the Issue

**Problem Description**

DrumBeatRepo's Angular frontend runs two test runners: Karma/Jasmine (`npm test`, the 15 `*.spec.ts` files) and Vitest (`npm run test-vitest`, the `*.spec.vite.ts` files). Only Karma can run Angular's `TestBed`. The Vitest setup has no Angular test environment, so a Vitest spec can only test plain classes built with `new`. Contributors can't write an Angular service test with dependency injection in Vitest.

**Expected Behavior**

A `*.spec.vite.ts` file can call `TestBed.configureTestingModule(...)` and `TestBed.inject(SomeService)` and run under `npm run test-vitest`, with the existing Karma and Vitest suites still passing. Per the owner, `npm test` stays on Karma, and one working example service test is the goal.

**Current Behavior**

The first `TestBed` call in a Vitest spec throws `Error: Need to call TestBed.initTestEnvironment() first`. None of the 5 existing Vitest specs uses `TestBed`; they all construct classes directly.

**Affected Components**

`frontend/vitest.config.ts` (no setup file, environment, or Angular plugin), a new Vitest setup file, `frontend/package.json` (dev dependencies only; the `test-vitest` script stays), one new example spec next to an existing service (`src/app/ui/services/space-bar-behaviour/player.events.service.spec.vite.ts`), and possibly the frontend README's testing section. The Karma setup (`karma.conf.js`, `angular.json` test target, the `*.spec.ts` files) and the Scala `engine/` are out of scope.

**Toolkit**

- Tool: Claude Code with my `CLAUDE.local.md` for the repo; I read the issue, the owner's reply, `package.json`, `vitest.config.ts`, `angular.json`, and the CI action `.github/actions/angular-test/action.yml`.
- What it produced: the split between the two runners, which files the issue touches, and the owner-confirmed scope (TestBed under the existing `test-vitest` script, `npm test` unchanged).
- Difficulties: there's no CONTRIBUTING.md, so I had to infer the rules from the README and CI; and the issue's acceptance criteria ("`npm test` runs using Vitest") conflicted with option A until the owner answered.
- What changed: I asked the question in my claim instead of guessing, then recorded the owner's answer in my memory file so later sessions follow it.

---

## Reproduction Process

**Environment Setup**

macOS on Apple Silicon; Node v24.22.1 and npm 11; Angular 22.0.1; Vitest 4.0.18 (`vitest/4.0.18 darwin-arm64 node-v24.22.1`). Code: my fork `akshay-sarkar/DrumBeatRepo` at `02d0367` (upstream `main` on 2026-10-22), set up with `cd frontend && npm ci`. I didn't build the Scala `engine/` (no `sbt fastLinkJS`); the Vitest run doesn't need it. Challenge: my default Node was v25.8.0, and `npm ci` printed `EBADENGINE` warnings for every Angular package (Angular 22 supports `^22.22.3 || ^24.15.0 || >=26.0.0`; CI uses 26). I installed `node@24` with Homebrew, put it first on `PATH` for that shell, and reinstalled `node_modules`, so an unsupported Node couldn't muddy the result. Second challenge: my first "no Vitest spec uses TestBed" check was a zsh-globbed `grep` that never actually ran, and a re-run matched my own scratch spec, so I switched to `git grep`, which only reads committed files.

**Steps to Reproduce**

1. `git clone https://github.com/Babali42/DrumBeatRepo.git && cd DrumBeatRepo && git checkout 02d0367 && cd frontend && npm ci`
2. Baseline (control): `npm run test-vitest -- run` passes 5 files / 30 tests, and `git grep -l "TestBed" -- '*.spec.vite.ts'; echo "exit $?"` prints `exit 1` (no Vitest spec uses TestBed).
3. Add `src/app/ui/services/space-bar-behaviour/player.events.service.spec.vite.ts`, a minimal spec mirroring the existing Karma spec: `TestBed.configureTestingModule({})`, `TestBed.inject(PlayerEventsService)`, `expect(service).toBeTruthy()`.
4. Run `npm run test-vitest -- run src/app/ui/services/space-bar-behaviour/player.events.service.spec.vite.ts`.
5. Observed: the test fails at `TestBed.configureTestingModule({})` with `Error: Need to call TestBed.initTestEnvironment() first`.

**Reproduction Evidence**

- **Commit showing reproduction:** none (the failing spec is a local scratch file on `02d0367`; I'll commit it as part of the fix)
- **Screenshots/logs:**

```
$ npm run test-vitest -- run            # baseline (trimmed)
 Test Files  5 passed (5)
      Tests  30 passed (30)

$ git grep -l "TestBed" -- '*.spec.vite.ts'; echo "exit $?"
exit 1

$ npm run test-vitest -- run src/app/ui/services/space-bar-behaviour/player.events.service.spec.vite.ts
     × should be created 2ms
 FAIL  src/app/ui/services/space-bar-behaviour/player.events.service.spec.vite.ts > PlayerEventsService (TestBed under Vitest) > should be created
Error: Need to call TestBed.initTestEnvironment() first
 ❯ TestBedImpl.get compiler [as compiler] node_modules/@angular/core/fesm2022/testing.mjs:1529:13
 ❯ TestBedImpl.configureTestingModule node_modules/@angular/core/fesm2022/testing.mjs:1436:10
 ❯ src/app/ui/services/space-bar-behaviour/player.events.service.spec.vite.ts:9:13
 Test Files  1 failed (1)
      Tests  1 failed (1)
```

- **My findings:** The gap is real today. Vitest runs plain-class specs fine, but `TestBed` fails before it does anything, because nothing initializes Angular's test environment under Vitest. My reading of `vitest.config.ts`, not yet verified by a fix, is that it needs at least a setup file calling `TestBed.initTestEnvironment`, and likely a DOM environment (`PlayerEventsService` listens on `document`) and an Angular compile step for decorators. Also: the `test-vitest` script already exists, so no script change is needed, which the owner confirmed.

**Toolkit**

- Tool: repro-check (my unit 2 skill, with the course's wild-repo scope), run on `claim-draft.md` + `repro-draft.md`.
- What it produced: accept, with all 7 required checks passing plus the preferred control-run (5 files / 30 tests passing, `git grep` exit 1). It also verified `02d0367` against upstream and noted that the cause was correctly hedged.
- Difficulties: its voice notes caught two claims without pasted output ("= upstream main", and "engine not needed" stated as fact) and a trimmed output block I hadn't marked as trimmed. It also suggested opening with a reply to the owner, which didn't apply because the course says not to post the repro on the issue.
- What changed: I dropped the unbacked "= upstream main", restated the engine point as what I actually did ("ran without building `engine/`"), marked the baseline as trimmed, and moved the hedge into the cause sentence.

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
