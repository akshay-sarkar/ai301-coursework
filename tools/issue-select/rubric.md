# Rubric: is this a good first issue?

Thresholds are measured against the capture date in eval mode and against today in live mode.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer_alive | Repo facts: "last 5 default-branch commits" (dates, authors, merged-PR names) and "maintainer first-response sample" | ALL of: (a) newest default-branch commit is within 90 days of the capture date; AND (b) at least one of the last 5 commits is by a non-bot author, or is a bot merging a human's pull request (message reads "Merge pull request #N from <human-username>/..."), or at least one issue in the response sample got a maintainer reply within 30 days. Commits that are only bot-authored merges of bot PRs (dependabot, automated bumps) with no 30-day maintainer reply in the sample fail (b). | required |
| repo_in_use | Repo facts: `archived:` flag, "last push to any branch", "latest release" | ALL of: not archived; last push within 90 days of capture; IF a release exists, the latest is within 12 months of capture ("none published" is not a failure). | required |
| nobody_on_it | Repo facts "this issue: assignees / linked PRs"; the Comments section | ALL of: assignees is none; no linked PR is open; no open PR is mentioned in the thread (a closed or merged-elsewhere PR does not count); no "I'll take this / working on this / /assign" comment within 60 days of capture unless a maintainer has told that person to stand down. Older claims with no PR, no assignee and no follow-up are abandoned and do not fail the check. A maintainer replying "just give it a try" to a claim is not an exclusive claim. If the thread and the sidebar disagree, believe the thread. | required |
| scope_bounded | Issue body and Comments | ALL of: not an umbrella/tracking issue (list of sub-items meant to be split); not a pure usage/support question; no unresolved design debate (a thread where maintainers disagree with each other or say the approach is undecided; a single issue body listing several possible causes or fix ideas is NOT a debate); no maintainer statement that the fix needs core/internal/architecture changes; not (open more than 2 years AND 2 or more closed-unmerged PR attempts in its history). One closed PR on an old issue does not fail. Terse bodies are fine. A bug reported by a maintainer with a stated cause is bounded. | required |
| issue_wanted | Issue header (opener username and association, labels) and Comments | ANY of: opened by a human with association OWNER, MEMBER, COLLABORATOR or CONTRIBUTOR; a maintainer comment agrees the change is wanted; label `good first issue` or `help wanted`. Fails when the opener is a bot account (username ends in `[bot]`) or has association NONE, AND there is no such label, AND no maintainer comment. | required |
| ai_policy_ok | Repo facts: "contribution policy" line | Fail only if the policy prohibits AI-assisted contributions. Disclosure, understanding/testing, and human-review conditions pass, and a ban limited to fully AI-generated work with assistive use allowed passes. No statement passes. | required |
| good_first_issue_label | Issue header labels | Label `good first issue` (or `help wanted` from a maintainer) is present. | preferred |
| maintainer_endorsed | Issue header (opener association) and Comments | Opened by an OWNER/MEMBER/COLLABORATOR, or a maintainer comment states they want the change or agrees with the proposed fix. | preferred |
| actionable_detail | Issue body | The body names the file/function to change or gives reproduction steps or acceptance criteria. | preferred |

## Verdict rule

Accept if and only if every `required` check passes. Any required `fail` rejects. An `unclear` on a required check counts as fail. `preferred` checks never change the verdict; they only rank accepted issues.
