# Evidence guide: where evidence lives in a plan package

An eval bundle has six parts, in this order: `## Repo facts`, `## Issue`, `## Thread highlights`, `## Repro evidence`, `## Candidate plan`, `## Candidate plan comment`. In live mode the issue side comes from the GitHub issue and its comment thread; the repro evidence is the student's posted repro comment (or the house repro pack quoted in the drafts); the candidate side is the student's `plan.md` and draft comment. The package is only what the drafts contain and quote.

## Diagnosis and grounding

- Where it lives: eval, the cause / diagnosis paragraph in `## Candidate plan`, read against every numbered step, control run, and pasted output in `## Repro evidence`. Live, the diagnosis section of `plan.md` against the student's posted repro comment.
- What good looks like: the blamed component, layer, or step is one the repro evidence does not show working. Every control run ("same input without -v parses fine", "the instance-method error still prints", "zeros already gone before the cast", "instant with bindings unchanged") is consistent with the cause. A plan that blames something a control already clears, or that adopts a confident thread diagnosis the evidence contradicts, fails however polished it is.

## Scope

- Where it lives: eval, the scope / in-scope / not-in-scope lines and the approach in `## Candidate plan`. Live, the Scope section of `plan.md`.
- What good looks like: one change aimed at the reproduced behavior, at the site the evidence isolates. Wider work (rewrites, migrations, new options, frameworks, CI or test-harness changes, other symptoms) is absent or named as deferred. Honestly scoping down ("deferring the Windows variant because I can't test it") is good. A plan that turns one fix into several fronts is a drive-by redesign, even if the core fix inside it is right.

## Executability

- Where it lives: eval, the files / areas and approach / order-of-work parts of `## Candidate plan`. Live, the Files and Approach sections of `plan.md`.
- What good looks like: named files or functions and one chosen approach, so a stranger could open the file and start. Red flags: no files named, "X or Y, whichever is easier", "not sure which layer", "somewhere", or a plan whose first step is to profile or investigate.

## Test plan

- Where it lives: eval, the test-plan part of `## Candidate plan`, read against the trigger and symptom in `## Repro evidence`. Live, the Test plan section of `plan.md` against the posted repro steps.
- What good looks like: re-run the reproduced trigger (or a regression test built from it) and name what will be observed after the fix, such as an exit code, an output line, a returned value, or a named test passing. "Run the full suite", "CI green", "should feel fast", or "nothing else should break" alone proves nothing about this fix.

## Honesty

- Where it lives: eval, the risks / unknowns part of `## Candidate plan` and any certainty or date words in the plan and the plan comment. Live, the Risks and unknowns and `## Deviations` sections of `plan.md`.
- What good looks like: what the author has not verified is labelled as an unknown or risk, with how it will be checked. No delivery dates, no "this will definitely fix it". After the build, any change from the plan is written under Deviations with what changed and why; "nothing changed" is stated in words, never left blank.

## Comms

- Where it lives: eval, `## Candidate plan comment` read against `## Thread highlights` (OWNER / MEMBER / COLLABORATOR notes, linked PRs, posted patches) and against the contribution-policy line in `## Repo facts`. Live, the draft comment against the issue thread and the repo's CONTRIBUTING.md, AI_POLICY.md, AGENTS.md.
- What good looks like: the comment engages what maintainers already said. It follows the culprit or direction they named, or says why the plan differs, and it acknowledges open PRs or posted patches instead of racing them. A docs-only workaround when the owner has isolated a code culprit and asked for testing is not thread-aware. On AI policy: treat the work as AI-assisted. If the repo requires disclosing all AI usage, the comment must name the tool and the extent of help. If no policy is stated, there is nothing to meet.
