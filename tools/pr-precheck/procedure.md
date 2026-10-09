# Procedure: how this tool grades a PR package

## Read order

1. Read the repo-facts block (live: `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and any CONTRIBUTING.md, AI_POLICY.md, or AGENTS.md at the root, in `docs/`, or in `.github/`). Write an ASKS list: every template section, every required checklist item or artifact (issue link, changelog/whatsnew entry, tests, removing a seeded bug's xfail marker), and the AI policy word for word with whether it requires disclosure. If the repo states none, write "no stated asks".
2. Read the issue. Write down the reported symptom in one line and the exact reproduction trigger (command, input, or call).
3. Read the thread highlights (live: the issue's comment thread). Write down every OWNER, MEMBER, or COLLABORATOR direction or question. If there are none, write "no maintainer direction".
4. Read the plan context (live: `plan.md`), including its deviation notes, BEFORE opening the PR. Write a PLAN list: every change the plan commits to, every file it names, everything it scopes out, every deferral, and each item in its test plan with the failure mode it targets. Reading the plan first matters: the diff and the description are graded against this list, so a confident description can't set the frame.
5. Read the diff (live: `git diff main...HEAD`, with `--stat`) and the commit list (live: `git log main..HEAD --oneline`). Read the diff before the description, so you know what the PR actually does before you read what it claims.
6. Read the test evidence (live: `test_evidence.md`).
7. Read the title and description last (live: `pr_draft.md`).

## Evidence gathering

1. For plan-fidelity: list every hunk in the diff (file, and what it changes in one line). Mark each "in PLAN", "in a deviation note", or "unplanned". Then walk the PLAN list and mark each planned change "present in diff", "deferred with a note", or "missing with no note".
2. For description-matches-diff: copy every claim the title and description make about what the PR changes, including fidelity claims ("exactly as planned", "no changes beyond…", "now documents…"). Next to each, write the hunk that delivers it, or "no such hunk". Mark a claim "no such hunk" only when the change it names is absent from the diff, not when the wording is a loose summary of a change that is there. Copy any deferral from the PLAN list and note whether the description states it.
3. For decisive-evidence: from the issue's reproduction (and any distinct failure mode the plan's test plan names), list each failure mode of the bug: each reported broken behavior with its trigger. A fix the plan applies to sibling code paths the repro doesn't exercise separately is not a separate failure mode. For each failure mode, find the evidence that re-runs it: the command, the before (it must quote the actual failure text, wrong value, or wrong output), and the after (pasted output, or the observed result stated against the same command). A newly added test that runs the failure mode and is shown passing after the fix counts as the after, as long as the before is shown too. Mark each "before and after shown", "statement only" (no command, or no quoted before), "different path" (the evidence runs a control, an unchanged path, or a different trigger than the issue's), or "not run". Do not list or grade the plan's controls or other items that check something that wasn't broken: they are not failure modes. Items that only say "the suite passes" or "nothing else breaks" belong to repo-checks-shown.
4. For clean-diff: scan every hunk. List any debug print or log, commented-out code or earlier attempt, dead or unused function, import reordering, whitespace or re-indent churn in code the fix doesn't touch, and any hunk that removes and re-adds identical lines without changing anything. Do not list unchanged lines that appear as removed and re-added inside a hunk that also makes the planned change; that is how the diff tool printed the edit, not debris. Read the commit messages too: "wip" or "cleanup while debugging" is a cue to look harder at the hunks, not debris on its own.
5. For standards-honored: walk the ASKS list. For each ask, note where the description or diff meets it, where the description explains why it isn't met, or "ignored". Check that each ticked checkbox is backed by evidence in the test evidence. Put each maintainer direction from read-order step 3 next to the description and note whether it is engaged.
6. For repo-checks-shown: copy the outcome line of each repo check (for Path Review, `make test-unit`, `make test-integration`, `make lint`, `make typecheck`) from the test evidence, or "not shown".

## Check execution

1. Run the checks in rubric order: plan-fidelity, description-matches-diff, decisive-evidence, clean-diff, standards-honored, repo-checks-shown.
2. Grade each check only from what you recorded in Evidence gathering. Re-read the package only to find a fact you failed to record, never to reconsider a recorded one.
3. plan-fidelity: fail if any hunk is "unplanned", or any planned change is "missing with no note". Otherwise pass.
4. description-matches-diff: fail if any claim has "no such hunk", if a fidelity claim is contradicted by an "unplanned" hunk, or if a recorded deferral isn't stated in the description. Otherwise pass.
5. decisive-evidence: pass only if every failure mode is "before and after shown". Any failure mode marked "statement only", "different path", or "not run" fails the check. Controls never decide this check.
6. clean-diff: fail if the debris list from gathering step 4 has any entry. Otherwise pass.
7. standards-honored: if the ASKS list is "no stated asks" and there is "no maintainer direction", pass. Otherwise fail if any ask is "ignored", any ticked box is unbacked, a required disclosure is absent, or a maintainer direction is unengaged.
8. repo-checks-shown: pass if at least one outcome line is shown and none is hidden. It is preferred, so report it either way.
9. If the evidence a required check needs is genuinely absent from the package (for example, there is no test evidence at all), grade it unclear and name what is missing. Never infer content the package doesn't contain.
10. Write one evidence line per check: the fact or quote that decided it.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required check is pass.
2. Count unclear on a required check as fail, except that standards-honored is pass when there are no stated asks and no maintainer direction.
3. Report repo-checks-shown, but leave it out of the verdict.
4. If the verdict is reject, the deciding check is the first failing required check in rubric order. Name it first in the summary and quote the evidence that decided it.
5. In live mode, add voice-guide notes and any procedure gaps after the check lines.
6. Emit the JSON block from SKILL.md last, with every check in rubric order, and nothing after it.
