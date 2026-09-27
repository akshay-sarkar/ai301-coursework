# Evidence guide: where proof lives in a reproduction package

A package has four parts: the issue context (title, body, thread highlights), the repo-facts block, the candidate claim comment, and the candidate repro report. In live mode the issue side comes from the GitHub issue thread and the repo's docs; the candidate side is the student's draft file(s). Pasted output is evidence; prose describing output is not.

## Environment

- Where it lives: eval, the repro report's environment line/section, compared with the version and platform in the issue body and the "bug reports" template asks in the repo-facts block. Live, the draft repro comment, compared with the issue body and the repo's issue template / SETUP docs.
- What good looks like: the report names the version (or commit SHA) and OS it ran on, and any setting the issue says matters (driver, build profile, shell, runtime version, config file). If the report's version or platform differs from the issue's, the report says so in words ("filed against 13.0.0; tested 15.2.0"). A difference that is only visible by comparing numbers yourself counts as silent. No environment record at all is a fail even when the artifact looks right, because nobody can place the attempt.

## Steps

- Where it lives: eval, the repro report's steps/commands and any input files it shows, compared with the "steps to reproduce", command, and input in the issue body (and any trigger note from a maintainer in the thread highlights). Live, the draft repro comment against the issue thread.
- What good looks like: a stranger with nothing but the comment could run it from a clean start to the trigger: exact commands, exact input contents, no private repos or unshared config. The command and input match the issue's character-for-character; check operators, separators, flags, range syntax, and expressions one by one. Any change is named with a reason. A step like "set up the project" or "use my config" is not followable.

## Behavior shown

- Where it lives: eval, the pasted output blocks, logs, exit codes, or screenshots in the repro report, read against the actual behavior described in the issue body. Live, the pasted output in the draft repro comment against the issue.
- What good looks like: the artifact shows the same symptom the issue reports: same error class and message, same exit code, same wrong output. Symptoms that are NOT the same: a graceful syntax/validation error vs a crash or panic; a compile error vs a runtime error; the program running normally (version banner, session list) vs the reported failure; a different exception from an older version. A cannot-reproduce is valid when the artifacts come from running the issue's exact trigger and show the symptom absent. A control run (a nearby input that works) strengthens the report but is not required.

## Honesty

- Where it lives: the analysis/conclusion/"actual" sections of the repro report and the claim comment, each sentence matched against the artifacts above.
- What good looks like: every claim points at something pasted. "Reproduced", "confirmed on 4.53.2", "ran it ten times", "I verified the race", "affects the current release" each need their own artifact; if it is not shown, it is a claim, not proof. Hypotheses are labelled as hypotheses. An honest cannot-reproduce that says what differed (environment, sizes, shell) and what a triggering setup might need is good proof. A confident report whose artifact shows something else, or that generalizes past the environments tested, fails.

## Comms

- Where it lives: the claim comment read against the issue; both comments read against the contribution-policy line in the repo-facts block (live: CONTRIBUTING.md, AI_POLICY.md, AGENTS.md, and the PR/issue templates).
- What good looks like: the claim names this issue's specifics (the error, file, or trigger) and a concrete next step such as reproducing or reading a named area; it promises investigation, not a fix or a date. Boilerplate ("assign me", "+1", "I can fix this in 2 days guaranteed") fails. On AI policy: treat the work as AI-assisted. If the repo requires disclosure of all AI usage (including comments or "any form"), a comment must name the tool and extent of help; if the policy only asks for disclosure in PRs, or asks that comments be in the contributor's own words, a human-voiced comment with no disclosure passes; if no policy is stated, there is nothing to meet.
