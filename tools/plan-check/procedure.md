# Procedure: how this skill grades a plan package

## Read order

1. Read the repo-facts block first (live: CONTRIBUTING.md, AI_POLICY.md and AGENTS.md at the repo root AND under `docs/` and `.github/`, plus the issue and PR templates). Write down the contribution-policy line word for word, and note whether it requires disclosing AI usage, and if so whether that covers comments or "any form" of use.
2. Read the issue next. Write down the reported symptom in one line and the exact trigger (command, input, or call) that produces it.
3. Read the thread highlights (live: the issue's comment thread). For every OWNER, MEMBER, or COLLABORATOR comment, write down any culprit they named (file, line, function), any direction they gave, any patch or binary they posted, and any linked or open PR. Separately, from any commenter (not only maintainers), write down every open or linked PR, posted patch, or test binary that targets this issue, with what it changes. A pushed branch or commit link on someone's fork that targets this issue counts as a posted patch. If there are no maintainer notes AND no PRs or patches, write "no maintainer direction or prior art".
4. Read the repro evidence before the plan (live: the student's posted repro comment, or the house repro pack quoted in the drafts). For each step, write what it ran and what it showed. Mark every control run or "works when X" observation as a CONTROL, and write which component, layer, or step it shows working correctly.
5. Only now read the candidate plan, then the candidate plan comment. Reading the evidence first matters: the diagnosis check must compare the plan's cause against what the evidence already pinned down, not let the plan's confident wording set the frame.

## Evidence gathering

1. For grounded-diagnosis: copy the plan's stated cause in one line (the component, layer, or step it blames). Put it next to the CONTROL list from read-order step 4. If the plan pastes new evidence of its own (output it ran after the posted repro), record it separately: it can support the approach, but the diagnosis is graded against the repro evidence, and new output never overrides a CONTROL.
2. For bounded-scope: list every distinct piece of work the plan commits to (each change, refactor, migration, new option, test-harness or CI change). Mark each one "fixes the reproduced behavior" or "beyond it". Separately list anything the plan names as deferred or out of scope.
3. For executable: copy the file(s), function(s), or area(s) the plan names, and the approach in one line. Note any approach that is still a choice ("or", "whichever", "not sure", "somewhere") and any first step that is "investigate", "profile", or "look into".
4. For decisive-test: copy each test-plan item and the outcome it expects. Mark whether it runs the reproduced trigger (or a test built from it) and whether the expected outcome is observable (exit code, output, value, passing test).
5. For thread-aware: put the maintainer notes and prior-art list from read-order step 3 next to the plan comment. Note whether the comment follows, names, or ignores each one. If the comment credits someone on the thread for a finding or a patch, check the credit against the thread (who posted it, and when) and record any mismatch.
6. For repo-conventions: put the policy note from read-order step 1 next to the plan comment. Note whether the comment contains an AI-use disclosure that names the tool and extent.
7. For honest-unknowns: copy the plan's risks/unknowns section, and any certainty words ("definitely", "guaranteed", "will fix", a date) in the plan or comment.

## Check execution

1. Run the checks in rubric order: grounded-diagnosis, bounded-scope, executable, decisive-test, thread-aware, repo-conventions, honest-unknowns.
2. For each check, apply its pass condition to the facts you recorded in Evidence gathering only. Re-read the package only if a needed fact was not recorded.
3. grounded-diagnosis: if any CONTROL shows the blamed component, layer, or step working, or shows the symptom present before the blamed step runs, grade fail, however confident or detailed the plan is. If the plan names no cause, grade fail.
4. bounded-scope: if any item is marked "beyond it" and is not named as deferred or out of scope, grade fail. Deferring part of the issue with a reason is a pass.
5. executable: if no file or area is named, the approach is still a choice, or the first step is investigation, grade fail.
6. decisive-test: grade pass only if at least one test item runs the reproduced trigger (or a test built from it) AND names an observable outcome. "Run the full suite" alone is a fail.
7. thread-aware: if read-order step 3 recorded "no maintainer direction or prior art", grade pass. Otherwise, grade fail if the plan comment ignores any recorded maintainer culprit, direction, or patch, or any recorded open PR or posted patch. A comment that names the PR or patch and says how the plan relates to it passes.
8. repo-conventions: if no AI policy is stated, grade pass. Grade fail only if the policy requires disclosure covering comments or any use and the comment has none.
9. honest-unknowns: grade it, but it is preferred.
10. If the evidence for a required check is genuinely missing from the package (for example, no test plan exists), grade unclear and say what is missing. Do not infer content the package does not contain.
11. Write one line of evidence per check: the fact or quote that decided it.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required check is pass.
2. Count unclear on a required check as fail, except the two exceptions the rubric names (thread-aware and repo-conventions pass when there is no maintainer direction or no AI policy).
3. Ignore honest-unknowns for the verdict. Report it.
4. If the verdict is reject, name the first failing required check in rubric order in the summary, and quote the evidence that decided it.
5. Emit the JSON block from SKILL.md last, with every check in rubric order and its one-line evidence.
