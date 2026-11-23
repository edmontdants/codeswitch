# Test scenarios: choosing the chat language at session start

Each scenario is a separate session. "Global instructions" means the user-level
always-loaded instructions file (for example `~/.claude/CLAUDE.md`), which
applies to every project. No project memory records any language.

For each scenario, write the agent's **first output to the user** exactly as it
would appear. If the agent would use a question tool, write the question and its
options. If the agent would write or change a file, say which file and the exact
line. Do not do the underlying task beyond what the first reply would contain.

## C1 — Native language known, user starts in English
Global instructions contain: "User's native language: Egyptian Arabic."
The user's first message: "check why the login endpoint returns 500 on staging"

## C2 — Native language known, user starts in their language
Global instructions contain: "User's native language: Egyptian Arabic."
The user's first message: "شوف ليه الـ login endpoint بيرجع 500 على الـ staging"

## C3 — Nothing known, user starts in English
Global instructions contain nothing about language.
The user's first message: "add pagination to the GET /orders endpoint"

## C4 — Later in a session where English was chosen
Global instructions contain: "User's native language: Egyptian Arabic."
At the start of this session the agent asked, and the user chose English. Ten turns later the user writes: "ok now run the tests"

## C5 — User names the language in the first message
Global instructions contain: "User's native language: Egyptian Arabic."
The user's first message: "reply in English please: summarize what this repo does"

## C6 — User answers the question from C3
Same session as C3. The agent asked which language to use; the user answers: "Arabic"
Write the agent's next output, including any file it would write.

## C7 — No one can answer
Global instructions contain: "User's native language: Egyptian Arabic."
This is a scheduled, non-interactive run. Its prompt: "summarize yesterday's failed CI runs"
