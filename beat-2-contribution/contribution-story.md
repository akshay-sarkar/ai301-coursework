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
**Pull Request:** https://github.com/Babali42/DrumBeatRepo/pull/623

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

On 2026-10-22 I added an Upstream section to my voice guide (claims, questions, pings, each with a wrong/right pair) and wired `check_voice.py` as a PreToolUse hook, which stopped `gh issue comment --help` in the wiring test as intended. What fell short: I had already posted my #611 claim through the GitHub website, so the guard never saw it; I ran `check_voice.py --draft` on the posted text afterwards (it passed) and now post through Claude Code. My `CLAUDE.local.md` for DrumBeatRepo records what took me a while to find: there's no CONTRIBUTING.md, so CI's `angular-test` action defines "all tests"; a `test-vitest` script already exists; Karma needs a Scala-built `engine/main.js`; and Angular 22 rejects my default Node 25. After the owner replied I updated it to say: reuse `test-vitest` and leave `npm test` alone.

**Unit 7: Installed the ship-gate, wrote a 3-check manifest from DrumBeatRepo's CI, and opened PR #623 after the gate passed**

On 2026-10-31 I wrote a manifest that runs `npm ci`, lint and `test-vitest-coverage` (from `.github/actions/angular-test` and the `angular-lint` action) and excludes Karma with a dated reason, because it needs the Scala-built `engine/main.js` and Chromium. Two things fought me: Homebrew upgrading `simdjson` broke both my default Node 25 and the `node@24` keg, so I moved to Node 26 (what CI uses) and dropped the PATH prefix from every row; and lint picked up the gitignored `coverage/` HTML report, so the lint row now runs `rm -rf coverage` and comes first. Before the gate, my voice guard stopped `pr_draft.md` four times on my own rules ("Nothing else in the file changes", "before" figures without pasted output, left-out items outside Notes for Reviewers, and "the five changes above are the five my plan listed"), and I fixed each line. The gate then passed (3/3 repo checks, pr-precheck accept on all 6), with one note my rubric can't fail: the plan promised to name the lockfile's transitive bumps and the description doesn't, which is the rubric gap I'd close next.

**Unit 8: Installed the PR monitor, wrote a playbook from DrumBeatRepo's real merge times, and rewrote its merged-PR draft into a short follow-up**

On 2026-11-05 I set my playbook's 5-day first-response threshold from the repo's 5 latest merged outside-contributor PRs (#625, #589, #583, #582, #578, all merged within 0–2 days), and its response and nudge rules come from my voice guide's Upstream section. `--state` read PR #623 as MERGED (7 checks passing, 5 thread items), and the full run drafted a "close the loop" move: a thanks, a conditional "if #611 didn't close automatically", and two follow-up items. What fell short: the monitor couldn't see the issue's state, so it hedged; I checked #611 myself, found it already closed, and cut that paragraph. I also cut the follow-up offers, because this contribution is done, and when my voice guard stopped the one-line thanks for naming nothing specific to the PR, I named `src/test-setup.vite.ts` and posted it through Claude Code.

---

## Why I Chose This Issue

Finding an issue took longer than fixing one. My scout's first scan looked promising (2 of 5 accepted), but both turned out to be wrong: one was already fixed on master, and the other only looked "wanted" because the repo's bug template stamps `good first issue` on everything. I patched my rubric for both. Then my first three picks from the Issue Selection site all got rejected, two because someone was already on them and one because the fix was already on main. After a while I noticed the pattern: popular good first issues get grabbed within days. So I stopped trusting labels and searched for issues with no assignee, no linked PR, and almost no comments. Eight more candidates still had a claim or an open PR. Babali42/DrumBeatRepo#611 was the one that was genuinely free, and my scout accepted it on all 7 required checks.

It's a good fit for me. It's TypeScript and test tooling, which is exactly what my profile says I want, and it's Angular instead of my usual React, which I wanted to learn. The owner offered two ways to do it. I picked the smaller one (get `TestBed` working in Vitest with one example) over a full Karma migration of about 15 spec files plus CI. My scout's Unit 6 difficulty note called it "ambitious" because the setup details and the `npm test` question were open, so I asked about `npm test` in my claim, and the owner answered the same day: reuse `test-vitest` and leave `npm test` alone. Honestly, the scout was more useful for what it ruled out than for what it found. Its verdicts held up when I checked the threads, and the fixes I made after scan 1 caught real cases later. But it couldn't judge size at first, and on the regrade it even rejected #611 because of my own claim comment, which I patched so it ignores the person running it.

**Claim comment:** https://github.com/Babali42/DrumBeatRepo/issues/611#issuecomment-6331477037

**Toolkit**

- Tool: issue-scout (my search strategy and rubric). I ran one scan, graded three site picks, then graded #611 directly. I found #611 by cross-checking the Issue Selection site against a GitHub search filtered to no assignee, no linked PR, and 0–1 comments.
- What it produced: scan 1 accepted 2 of 5 (both false positives); the three site picks were all rejected on one required check each; #611 was accepted on 7/7 required and 3/3 preferred checks.
- Difficulties: scan 1 couldn't see bugs already fixed on master or labels added by issue templates, and passed a one-commit practice repo (logixia). Popular good first issues were almost always claimed or had an open PR, which the ranking stage can't see.
- What changed: I added `not_already_fixed`, a template-label rule in `issue_wanted`, and a repo-history clause in `repo_in_use`; I bounded query 1 by date and issue age. In Unit 6 I replaced `scope_bounded` with `scope-feasible` plus a difficulty note (which flagged #611 as ambitious), and patched `nobody_on_it` after it rejected #611 for my own claim.

---

## Understanding the Issue

**Problem Description**

The frontend has two test runners side by side: Karma, which runs the older `*.spec.ts` files through `npm test`, and Vitest, which runs the newer `*.spec.vite.ts` files through `npm run test-vitest`. The catch is that only Karma knows how to run Angular's `TestBed`. Vitest was set up for plain TypeScript classes, so if you want to test an Angular service with dependency injection, you're stuck writing it for Karma. The owner wants new tests to be able to go to Vitest.

**Expected Behavior**

I should be able to write a `*.spec.vite.ts` file that calls `TestBed.configureTestingModule(...)` and `TestBed.inject(SomeService)`, run it with `npm run test-vitest`, and have it pass, without breaking any of the existing Karma or Vitest tests. The owner was clear that `npm test` stays on Karma for now and that one working example is the goal.

**Current Behavior**

The moment a Vitest spec touches `TestBed`, it fails with `Error: Need to call TestBed.initTestEnvironment() first`. That's also why none of the 5 existing Vitest specs use it. They all just create their classes with `new`.

**Affected Components**

Mostly `frontend/vitest.config.ts`, which has no setup file and no browser-like environment, plus a new setup file to initialize Angular's testing, a dev dependency in `frontend/package.json`, one example spec next to `PlayerEventsService`, and a line in the README. I'm not touching Karma's side (`karma.conf.js`, `angular.json`, the `*.spec.ts` files) or the Scala `engine/`.

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

`npm run test-vitest` runs Vitest with `frontend/vitest.config.ts`, which only sets `include: ['./**/*.spec.vite.ts']`, `globals: true` and coverage. It has no `setupFiles` and no `environment`, so two things Angular's `TestBed` needs never happen. First, nothing calls `getTestBed().initTestEnvironment(...)`. Under Karma, the `@angular/build:karma` builder does that, but under Vitest nobody does, so the first `TestBed.configureTestingModule({})` throws `Need to call TestBed.initTestEnvironment() first` (my repro on `02d0367`). Second, Vitest's default environment is Node, which has no `document`. Angular's browser testing platform injects `DOCUMENT`, and `PlayerEventsService` listens to `fromEvent(document, 'keydown')`. The repro's control shows Vitest itself works: the 5 existing specs pass (30 tests), but none uses `TestBed` (`git grep` exit 1), because they test plain classes built with `new`.

**Proposed Solution**

Add the missing test environment to the Vitest setup and prove it with one example, the owner's "make the TestBed and the first try":
- a new `frontend/src/test-setup.vite.ts` that imports `@angular/compiler` and calls `getTestBed().initTestEnvironment(BrowserTestingModule, platformBrowserTesting())`;
- `environment: 'jsdom'` and `setupFiles` in `vitest.config.ts`;
- `jsdom` as a devDependency;
- `player.events.service.spec.vite.ts` as the first `TestBed` example, mirroring the three cases of its Karma spec;
- one README line under "How to test it".

I'm deliberately NOT changing:
- `npm test` or the `test-vitest` scripts (the owner asked me to reuse `test-vitest`);
- any Karma `*.spec.ts`, including the sequencer tests the owner left for later;
- `karma.conf.js`, `angular.json`, `tsconfig.spec.json`, CI, or the Scala `engine/`.

I'm also not adding zone.js or the Analog Vitest plugin, which I expect a service test won't need. Components are out of scope.

**Implementation Plan**

1. Sync my fork with upstream `main` (`9ea0e66`; nothing since `02d0367` touches Vitest) and branch `test/611-vitest-testbed`.
2. Add `src/test-setup.vite.ts`, and point `vitest.config.ts` at it with `environment: 'jsdom'`.
3. `npm install --save-dev jsdom@^30.1.3`.
4. Add `player.events.service.spec.vite.ts` with the three TestBed cases (created; Space keydown emits `'toggle'`; other keys don't emit).
5. Add the README line.
6. Re-run the repro spec, `npm run test-vitest-coverage` and `npm run lint`, and save the output to `test_evidence.md`.

Deviations: no change to the code plan. The diff is the planned 6 files in 3 commits (`04e22b5` setup + config + `jsdom`, `cba3dc3` example spec, `2faea70` README). The setup file plus `jsdom` was enough, so no zone.js or Analog plugin was needed. Two environment differences: I branched from the updated upstream `main` (`9ea0e66`, synced 2026-10-31) instead of `02d0367`, and I built on Node 26.13.0 instead of Node 24 after Homebrew broke the `node@24` keg. Both are recorded in `plan.md` and in the PR's Notes for Reviewers.

**Toolkit**

- Tool: plan-check (my unit 3 skill with the wild-repo scope), run on `plan.md` + `plan-comment.md`. Before writing the plan, I had Claude prototype the setup in a throwaway clone, so the plan rests on a fix I'd seen work rather than a guess.
- What it produced: accept, with all 6 required checks passing. It confirmed the cause matches the repro error and the control, and that the plan follows all three points of the owner's reply (reuse `test-vitest`, "the TestBed and the first try", sequencer tests later).
- Difficulties: its voice notes caught that my plan comment said "reproduced on `02d0367`" with no output to point to (I'm not posting a repro comment), and that "doesn't need zone.js/Analog" was stated as fact before my own run. It also found that npm was crashing on my machine (`libsimdjson.30.dylib` missing after Homebrew upgraded `simdjson` while installing `node@24`), and it flagged two procedure gaps: no rule for wild issues without a posted repro, and no check that a pinned package version exists.
- What changed: I pasted the 4-line error into the comment, moved the zone.js/Analog claim into Risks, and reinstalled Node. I first pinned Node 24 in my shell profile and manifest, then switched to Homebrew's default Node 26 (CI's version) when the reinstall broke the `node@24` keg.

---

## Testing Strategy

Three observable checks prove the fix, all run on Node 26.13.0 (the major version CI uses).

1. **The repro trigger, before and after.** `npm run test-vitest -- run src/app/ui/services/space-bar-behaviour/player.events.service.spec.vite.ts`. Before (`main` at `02d0367`), it fails with `Error: Need to call TestBed.initTestEnvironment() first` and `Tests 1 failed (1)`. After (branch), `Test Files 1 passed (1)` and `Tests 3 passed (3)`. The three tests are: the service is created via `TestBed.inject`; a Space keydown dispatched on `document` emits `'toggle'` (this needs jsdom's `document`, so it proves the environment, not just the setup file); and other keys don't emit.
2. **The whole Vitest suite with coverage, as a control.** Before (`main` at `9ea0e66`): `Test Files 5 passed (5)`, `Tests 30 passed (30)`. After: `Test Files 6 passed (6)`, `Tests 33 passed (33)`, coverage table written (`All files | 88.28 | 64.28 | 78.94 | 88.57`). This shows the new jsdom environment and setup file didn't break any existing plain-class spec.
3. **Lint, compared with `main`.** `npm run lint` gives `0 errors and 3 warnings` on both `main` and the branch (limit `--max-warnings=30`). The 3 warnings are in `audio-engine.adapter.ts` and `sequencer.service.ts`, which I didn't touch.

Not run locally: Karma (`npm run test-ci`), which needs the Scala-built `engine/main.js` and Chromium. My diff adds no `*.spec.ts` and doesn't change Karma's config (`tsconfig.spec.json` only includes `src/**/*.spec.ts`), so CI's `angular-test` job is the check there. My ship-gate ran checks 2 and 3 plus `npm ci` from a fresh shell and passed (`ok (6s)`, `ok (3s)`, `ok (6s)`).

**Toolkit**

- Tool: my ship-manifest (three CI-sourced checks, Karma excluded with a dated reason), run by hand and then by the ship-gate; outputs saved in `test_evidence.md`.
- What it produced: before/after for the trigger (1 failed to 3 passed), the suite going from 30 to 33 passing tests with coverage intact, and identical lint counts on `main` and the branch.
- Difficulties: my first "no TestBed in any Vitest spec" check was a zsh-globbed `grep` that never ran; `npx eslint .` listed the gitignored `coverage/` HTML report as if it were source; and my baseline Node changed mid-unit (24 to 26) when Homebrew broke the keg.
- What changed: I switched to `git grep` (committed files only), made the lint row clear `coverage/` first, and re-ran the `main` baseline on Node 26 so before and after use the same runtime for the suite and lint.

---

## Implementation Notes

**2026-10-22**

Claimed #611 (option A). The owner replied the same day: reuse `test-vitest`, aim for "the TestBed and the first try", leave the sequencer Karma tests for later. Forked and reproduced on `02d0367` under Node 24 (`Need to call TestBed.initTestEnvironment() first`). Found that `test-vitest` already exists, so no script change is needed.

**2026-10-31**

Synced my fork to upstream `main` (`9ea0e66`), checked that nothing since `02d0367` touches Vitest, and branched `test/611-vitest-testbed`. Decision: no zone.js and no Analog plugin, because Claude's prototype in a throwaway clone showed a setup file plus `jsdom` was enough for a service. Wrong turn: adding only the setup file moved the error to Angular's `DOCUMENT` injection, so `environment: 'jsdom'` was needed too. Environment detour: Homebrew's `simdjson` upgrade broke `npm` on Node 25 and then the `node@24` keg, so I rebuilt and tested on Node 26 (CI's version) and re-ran the `main` baseline there (still 5 files / 30 tests).

**2026-10-31**

Built in 3 commits through Claude Code, reading each diff: `04e22b5` (setup file, `vitest.config.ts`, `jsdom`), `cba3dc3` (the three-case `PlayerEventsService` spec, using `firstValueFrom` instead of Jasmine's `done`), and `2faea70` (README line). Decision: commit the lockfile exactly as `npm install --save-dev jsdom@^30.1.3` produced it (+507/-8, transitive `parse5`/`entities` updates), not hand-edited. Ran the evidence, iterated `pr_draft.md` through the voice guard until it passed, ran the ship-gate (PASS), pushed, and opened PR #623.

**Toolkit**

- Tool: Claude Code with my `CLAUDE.local.md`, for the edits (one prompt with the five exact changes, then I reviewed the diff); my voice guard and ship-gate hooks before the PR.
- What it produced: a 6-file, 3-commit branch matching the plan, and a reviewer-facing note from Claude worth checking (the 8 deleted lockfile lines, the `git add -N` it ran to show new files in the diff, and the unsubscribe placement in the KeyA test).
- Difficulties: Claude ran `git add -N` to show the new files, which I had to undo before committing; the Homebrew/Node breakage cost the most time; and `git log` output kept disappearing into the pager.
- What changed: I reset the index before staging each commit by hand, pinned my shell to Homebrew's default Node 26, and used `git --no-pager` for the evidence commands.

---

## Pull Request

**PR Link:** https://github.com/Babali42/DrumBeatRepo/pull/623

**PR Description:** "test: run Angular TestBed specs under Vitest", `Fixes #611`. It explains that `vitest.config.ts` had no setup file (so nothing called `TestBed.initTestEnvironment()`) and no DOM environment, and lists the five changes: `src/test-setup.vite.ts`, `environment: 'jsdom'` + `setupFiles` in `vitest.config.ts`, `jsdom` as a devDependency, the `PlayerEventsService` TestBed spec, and a README line. Testing pastes the trigger before (`Need to call TestBed.initTestEnvironment() first`) and after (`Tests 3 passed (3)`), the suite before/after (30 to 33 passing), and lint on `main` and the branch (same 3 warnings), and says Karma wasn't run locally and why. Notes for Reviewers lists what's left out on purpose (`npm test`, Karma specs and config, CI, `engine/`, components), the plan differences (branched from `9ea0e66`, built on Node 26), and how I used AI.

**Maintainer Feedback:**

- 2026-10-31: no maintainer review yet. CI passed on the PR (https://github.com/Babali42/DrumBeatRepo/actions/runs/41251300883), including the Karma job I couldn't run locally, which confirms the change didn't affect the Karma specs.
- 2026-10-31: CodeRabbit (automated review) passed 4 checks and warned on one: its "Linked Issues" check said the issue's acceptance criterion "`npm test` runs using Vitest" isn't met because `npm test` still runs Karma. That was a deliberate choice: the owner told me on the issue to reuse `test-vitest`, and my PR's Notes for Reviewers says `npm test` is left out on purpose. Codecov reported all modified and coverable lines covered.
- 2026-10-31: The owner, @Babali42, thanked me ("really helpful because unit test infrastructure can be time consuming"), said he'll reuse the setup with a test in a future feature (translating drum part names, e.g. kick to "grosse caisse"), and welcomed the added coverage. He also said that at this level of quality he didn't need the AI disclosure, because the work was well tested and understood, and that he's been receiving "AI flood PRs". He merged the PR as `d8f78fb` and added me as a contributor for code and test via all-contributors (PR #624). No changes were requested, so there was nothing to address.
- 2026-11-05: My PR monitor drafted a merged-PR follow-up with a conditional "if #611 didn't close automatically" paragraph. I checked the issue, found it already closed, and dropped that part. I also dropped the follow-up offers, since I'm not taking on more work here, and posted a one-line thanks for the merge and the contributor credit: https://github.com/Babali42/DrumBeatRepo/pull/623#issuecomment-6674004279

**Status:** Merged (2026-10-31, `d8f78fb`)

**Toolkit**

- Tool: the ship-gate hook (`check_gate.py --branch`) running my 3-check manifest and my unit 4 pr-precheck in live mode; my voice guard on `pr_draft.md`; then `gh pr create` through Claude Code so both hooks saw it.
- What it produced: `ship-gate: PASS` (3/3 repo checks; pr-precheck accept on plan-fidelity, description-matches-diff, decisive-evidence, clean-diff and standards-honored, plus repo-checks-shown), then PR #623 opened on the upstream repo, not my fork.
- Difficulties: the hook test showed only the ship-gate's stop, not the voice guard's (both are registered in `settings.json`, so Claude Code probably reported only the first block). The voice guard stopped my draft four times on my own PR rules, and pr-precheck flagged a plan promise (naming the lockfile's transitive bumps) that my rubric can't fail.
- What changed: I rewrote each flagged line in `pr_draft.md` instead of loosening the guard. I kept the gate's 30-minute pass by not editing the draft afterwards, and noted a new rubric check to add: a plan promise that isn't met and isn't recorded as a deferral.

---

## Learnings and Reflections

**Technical Skills Gained**

- How Angular's `TestBed` actually gets initialized. Karma's `@angular/build:karma` builder calls `initTestEnvironment` for you, so I'd never had to think about it. Under Vitest I wrote that one line myself (`getTestBed().initTestEnvironment(BrowserTestingModule, platformBrowserTesting())`, plus `import '@angular/compiler'` for JIT), and learned that the browser testing platform also needs a DOM, which is why `environment: 'jsdom'` was the second half of the fix.
- Reading a repo's real rules from CI when there's no CONTRIBUTING.md. `.github/actions/angular-test/action.yml` told me exactly which commands "pass all tests" meant (Karma headless, then Vitest with coverage, then Codecov), and became the rows of my ship manifest.
- Practical git hygiene on a fork: `gh repo fork --clone` for `origin`/`upstream`, fast-forwarding my fork to upstream before branching, `.git/info/exclude` for working files that must never be committed, `git grep` to search only committed files, and `gh repo set-default` so the PR opened upstream, not on my fork.
- Writing evidence a reviewer can check: the same command before and after, a control run, and the same check (lint) run on `main` and the branch, instead of "tests pass".

**Challenges Overcome**

- Finding an issue nobody had already taken. My scout's first scan and my first three site picks all failed, mostly on `nobody_on_it` or `not_already_fixed`. I switched to cross-checking the site's list against a GitHub search for issues with no assignee, no linked PR and 0–1 comments, and #611 was the one that held up.
- An ambiguous acceptance criterion. The issue said "`npm test` runs using Vitest", which didn't fit the small option. I asked the owner one concrete question in my claim instead of guessing, and he answered the same day: reuse `test-vitest`, leave `npm test` on Karma.
- My machine fighting me. Angular 22 rejected my Node 25; installing `node@24` upgraded `simdjson`, which broke Node 25's npm; reinstalling Node then broke `node@24`. I ended on Homebrew's default Node 26, which is also what CI uses, and recorded the change as a deviation.
- My own tools pushing back. The voice guard stopped my PR description four times on my own rules (an "exactly as planned" style line, "before" numbers with no pasted output, left-out items outside Notes for Reviewers). Each stop was right, and the final description was better for it.

**What I'd Do Differently Next Time**

- Check the toolchain first. I'd confirm the repo's supported Node range and match CI's version on day one, before installing anything, instead of discovering the mismatch through `EBADENGINE` warnings mid-repro.
- Write the PR description against my voice guide from the start, rather than drafting freely and letting the guard catch fidelity claims. Most of the stops were the same kind of mistake.
- Check a tool's hedges against the source before acting on them. My PR monitor couldn't see whether #611 had closed, and an automated page fetch I tried returned a stale copy that still showed it open. Checking the live issue myself settled it.
- Keep disclosing AI use even when a maintainer says it isn't needed. The owner said my disclosure was unnecessary at this quality, but he also mentioned getting "AI flood PRs". Being specific about what the AI did and what I checked is part of what made this PR different from those.

---

## Resources Used

- The issue thread itself, especially the owner's reply on 2026-10-22, which settled the scope (reuse `test-vitest`, "the TestBed and the first try", sequencer tests later): https://github.com/Babali42/DrumBeatRepo/issues/611
- DrumBeatRepo's CI action, which defined "pass all tests" for my manifest: `.github/actions/angular-test/action.yml` (https://github.com/Babali42/DrumBeatRepo/tree/main/.github/actions)
- DrumBeatRepo's README ("How to test it" and the engine build steps), the only contributing guidance in the repo: https://github.com/Babali42/DrumBeatRepo#readme
- Angular's error message and stack trace (`Need to call TestBed.initTestEnvironment() first`, from `@angular/core/fesm2022/testing.mjs`), which pointed straight at the missing setup call. The next error, Angular asking for `DOCUMENT` once the setup file was in, pointed at the missing DOM environment.
- Claude (Anthropic), via Claude Code, for prototyping the setup, making the edits from my plan, and running my course tools (issue-scout, repro-check, plan-check, pr-precheck, voice guard, ship-gate, PR monitor).
