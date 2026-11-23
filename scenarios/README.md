# Test scenarios

These are the scenarios used to build the skill test-first: each version was
run on fresh agents without the change (the baseline), then with it.

| File | What it covers |
|---|---|
| [`scenarios-hr-arabic.md`](scenarios-hr-arabic.md) | Egyptian Arabic user, HR codebase. S1 domain terms in a bug explanation, S2 staff-facing handbook and `ar.json`, S3 relaying an English subagent report, S4 commit, title and task item, S5 HR gap analysis that must stay English |
| [`scenarios-multilingual.md`](scenarios-multilingual.md) | A1 Spanish, A2 Japanese, A3 romanized Hindi, A4 French staff content, A5 Egyptian Arabic, A6 German project instructions for docs, A7 Japanese and A8 Korean in an unrelated domain (loanword script) |

## How to run

For each rep, start a fresh agent with no other language instructions and give it:

```text
Read skills/code-switching/SKILL.md and treat it as a skill already loaded in your session.
Read scenarios/<scenario file>. Each scenario is a separate session with a different user.
Write exactly what you would produce for each scenario. Output only the results, one heading per scenario.
```

Run at least 3 reps per version. For a baseline, run the same prompt with the
previous version of the skill, or with no skill.

## What passes

- The chat reply is in the user's language, dialect and script.
- Domain and code terms are in English, in backticks, spelled as in the code. They get a gloss on first mention only, and the same name is used every time.
- Everyday words are not paired with English.
- Loanwords use the form local developers write (Japanese テスト, Korean 테스트).
- Staff-facing content and locale strings are in the readers' language (S2, A4).
- Specs, analyses, commits, titles and task items are in English, unless the project's instructions set another language (A6).
- Tool output is quoted exactly as it is.

## Results for v1.0.0 (3 reps each)

| Check | Previous version | v1.0.0 |
|---|---|---|
| Gloss on first mention only (S1) | 0/3 | 3/3 |
| Staff handbook in the readers' language (S2) | 0/3 | 3/3 |
| HR gap analysis stays English (S5) | not run | 3/3 |
| Japanese and Korean loanword script (A2, A7, A8) | 0/3 (A2), 0/1 (A7, A8) | 3/3 |
| Spanish, Hinglish, French, German, Arabic (A1, A3-A6) | 2/2 | 3/3 |

Limitations: every run used one model family, scoring was done by reading the
outputs by hand, and the scenarios are short, single-turn sessions.
