# Getting Angular's TestBed to run under Vitest in DrumBeatRepo

## Overview

DrumBeatRepo is an open-source drum-beat web app with an Angular frontend and a Scala engine. Its frontend runs two test runners side by side: the older Karma setup and a newer Vitest setup. The owner wanted new tests to move to Vitest, but Vitest could not run Angular's `TestBed`, the standard tool for testing Angular services. Any test that needed dependency injection still had to be written for Karma. In issue #611 I added the missing test environment to the Vitest setup and wrote the first working `TestBed` example. My pull request, [Babali42/DrumBeatRepo#623](https://github.com/Babali42/DrumBeatRepo/pull/623), was merged on the day I opened it, and the owner added me to the project as a contributor for code and tests.

## What happened

**Finding the issue (Oct 22).** My course tool for scouting issues kept turning up good first issues that someone had already claimed, or that were already fixed. So I searched for issues with no assignee, no linked pull request and almost no comments. Issue #611 passed every check my scout runs. The owner offered two options: set up Vitest so it can run `TestBed`, or move every test off Karma. I took the smaller option. In my claim I asked one question, because the issue said `npm test` should run Vitest and that didn't fit the small option. The owner answered the same day:

> I already have a non run test-vitest script (see frontend/package.json) with a filter on .spec.vite.ts so you can re-use this. The aim is really to make the TestBed and the first try.

That set my scope: keep `npm test` on Karma, use the existing `test-vitest` script, and get one example working.

**Reproducing it (Oct 22).** I wrote a minimal spec for an existing service, `PlayerEventsService`, that mirrors its Karma spec, and ran it under Vitest on `main` at `02d0367`. It failed on the first `TestBed` call:

```
$ npm run test-vitest -- run src/app/ui/services/space-bar-behaviour/player.events.service.spec.vite.ts
     × should be created 2ms
Error: Need to call TestBed.initTestEnvironment() first
 ❯ TestBedImpl.configureTestingModule node_modules/@angular/core/fesm2022/testing.mjs:1436:10
 Test Files  1 failed (1)
      Tests  1 failed (1)
```

As a control, the 5 existing Vitest specs still passed (30 tests), and `git grep -l "TestBed" -- '*.spec.vite.ts'` returned exit 1. Vitest itself worked. It just had never been set up for Angular, because none of the existing specs used `TestBed`.

**Finding the cause.** `vitest.config.ts` had no setup file and no environment. Under Karma, Angular's builder calls `initTestEnvironment` for you, so nobody had needed to think about it. Under Vitest, nobody calls it. Vitest's default environment is also plain Node, with no `document`, and Angular's browser testing platform needs one. Before I wrote my plan, a quick prototype in a throwaway clone confirmed this. With only a setup file, the first error went away and a second one appeared: Angular could not inject `DOCUMENT`. Adding a DOM environment fixed that too.

**Building it (Oct 31).** The fix is small. A new setup file initializes Angular's test environment:

```ts
// frontend/src/test-setup.vite.ts
import '@angular/compiler';
import { getTestBed } from '@angular/core/testing';
import { BrowserTestingModule, platformBrowserTesting } from '@angular/platform-browser/testing';

getTestBed().initTestEnvironment(BrowserTestingModule, platformBrowserTesting());
```

The Vitest config points at it and switches to `jsdom`, which I added as a dev dependency:

```diff
     globals: true,
+    environment: 'jsdom',
+    setupFiles: ['./src/test-setup.vite.ts'],
```

I also ported the three cases of the `PlayerEventsService` Karma spec to Vitest. One of them only passes if the DOM environment is real, because it dispatches a key press on `document`:

```ts
it('should emit "toggle" when Space key is pressed', async () => {
  const emitted = firstValueFrom(service.playPause$);
  document.dispatchEvent(new KeyboardEvent('keydown', { code: 'Space' }));
  await expect(emitted).resolves.toBe('toggle');
});
```

With the fix in, the same command that failed now passed, and the full suite went from 30 to 33 passing tests with coverage still reported:

```
$ npm run test-vitest -- run src/app/ui/services/space-bar-behaviour/player.events.service.spec.vite.ts
 Test Files  1 passed (1)
      Tests  3 passed (3)

$ npm run test-vitest-coverage
 Test Files  6 passed (6)
      Tests  33 passed (33)
All files          |   88.28 |    64.28 |   78.94 |   88.57 |
```

Lint showed the same 3 warnings on `main` and on my branch, so I added none.

**Changing my pull request description.** Before opening the PR, I ran my description through a voice guard I built in the course. It checks my writing against rules in my own voice guide. It stopped the description four times. The first stop quoted this line:

> voice-guard: STOP. your own rule: "I don't write "exactly as planned" or "no other changes". The diff shows that, and if it doesn't, the sentence is a lie.". Draft line: "`frontend/vitest.config.ts`: adds `environment: 'jsdom'` and `setupFiles: ['./src/test-setup.vite.ts']`. Nothing else in the file changes."

I removed "Nothing else in the file changes", since the diff already shows it. The other stops made me paste the "before" test numbers instead of only stating them, and move the list of things I left out into a Notes for Reviewers section. The description I opened was shorter, and every claim in it had output behind it.

**The result (Oct 31).** CI passed on the PR, including the Karma job I couldn't run on my machine. The owner merged it as `d8f78fb` the same day and wrote that it was "really helpful because unit test infrastructure can be time consuming". He plans to reuse the setup for a future feature that translates drum part names. He also added me as a contributor for code and tests.

## Limitations and what's next

The fix covers Angular services only. I didn't try a component that loads a separate template or stylesheet (`templateUrl` or `styleUrl`), and those may need an extra compile step under Vitest, such as the Analog plugin. The 15 Karma specs are also still on Karma, which is what the owner asked for, but it means the project still runs two test runners. CodeRabbit, an automated reviewer on the PR, flagged that the issue's original text asked for `npm test` to run Vitest. I followed the owner's later answer instead, but that gap is still open.

If I took a next step, it would be one component spec under Vitest, to find out whether the setup needs an Angular compile step for templates. That answer decides whether the remaining Karma specs, including the sequencer component tests the owner called tricky, can move to Vitest at all.
