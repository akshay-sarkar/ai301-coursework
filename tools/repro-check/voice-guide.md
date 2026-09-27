# Voice guide: how I talk upstream

## Who I am in threads

I'm a senior full-stack developer (React, TypeScript, Node) contributing to a Python codebase as a first-time contributor here. I'm in this repo to reproduce and fix one scoped bug. Readers can expect exact commands, pasted output, and plain statements of what I did and didn't verify.

## Rules I write by

### Rule: Promise the investigation, not the fix

In a claim, I commit only to the next thing I will actually do. No fix, no PR, no date until I've reproduced it.

- Wrong: "I'll have a fix for verify_password up by tomorrow."
- Right: "I'm going to reproduce this locally against the H-05 test and post what I see."

### Rule: Name the issue's specifics

Every comment names something only this issue has: the function, the error, the file, or the test.

- Wrong: "Hi, I'd like to work on this issue, please assign me."
- Right: "I'd like to take this: `verify_password` in core/security.py raises `UnknownHashError` on a non-bcrypt hash instead of returning False."

### Rule: Paste it or don't say it

If I claim I ran something, the output is in the comment. Anything I didn't run is labelled as a guess.

- Wrong: "Confirmed on Python 3.12 as well, and the root cause is passlib."
- Right: "Tested on Python 3.11.9 (output below). I haven't tried 3.12. My guess is passlib's `CryptContext.verify` raises before bcrypt is called, not yet verified."

### Rule: Same trigger as the issue

I run the issue's exact input first. If I change anything, I say what and why.

- Wrong: "Ran it with an empty hash and got an error, so it reproduces."
- Right: "Ran the test's exact input, `verify_password(\"password\", \"not_a_valid_bcrypt_hash\")`; output below."

### Rule: Plain register, no hype

No "rigorous", "complete", "guaranteed", or "100%". State what happened.

- Wrong: "I've done a complete and rigorous reproduction and I'm confident about the cause."
- Right: "Reproduced on main at 2f4e82f; traceback below."

## Things I never post

- A fix promise or delivery date in a claim comment.
- "+1", "same here", or "same as above, can confirm" in place of my own repro.
- A result I didn't paste output for.
- Hype words: rigorous, guaranteed, 100%, definitely.
- A root cause stated as fact before I've traced it.
- Output from a different input than the issue's without saying so.
