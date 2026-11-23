# codeswitch

**Talk in your language. Ship in English.**

`codeswitch` is an agent skill for developers whose first language is not
English. Your coding agent talks to you in your own language, so you read its
replies faster. It keeps technical and domain terms in English, spelled exactly
as your code names them, and it keeps the things that need English (code,
specs, docs, commits, prompts for other agents) in English.

In linguistics, *code-switching* means moving between two languages in one
conversation. Bilingual developers already do this when they talk about work.
This skill teaches your agent to do it the same way.

## Why

- **You read your native language faster.** Long agent replies in a second language slow you down, especially at the end of a long day.
- **Models and tools work best in English.** Specs, docs, commit messages and prompts for other agents should stay in English.
- **Translated domain terms cause confusion.** If `LeaveRequest` is called by three different translated names, nobody knows which is the real one. Your code already uses one name for each concept (the Ubiquitous Language). The agent should use that same name.

## What it does

At the start of each session, the agent asks once: **English, or your native language?** You choose per session, because some days you want English. Your choice lasts until the session ends. The agent skips the question in three cases:

- Your first message is already in your language.
- Your first message names a language.
- No one is there to answer, for example in a scheduled run.

Your native language is saved once, in your user-level instructions file, so every project knows it. Your per-session choice is never saved.

Before the agent writes anything, it decides who will read it:

| Output | Who reads it | Language |
|---|---|---|
| Replies, progress updates, questions, summaries | You, right now | Your language, with English terms |
| Code, comments, tests, specs, plans, docs, commits, PRs, prompts for other agents | Engineers, tools, other agents | English |
| UI strings, locale files, content for staff or end users, messages sent for you | Other people | The readers' language |
| Command output, logs, errors, diffs | Anyone | Kept exactly as it is |
| Session titles, branch names, task lists | Tools | Short English |

In replies to you, there are three kinds of words:

- **Domain and code terms** (`LeaveRequest`, `Pending`, `Manager`) stay in English, spelled exactly as in the code. The first time a term appears, the agent adds a short explanation in your language. After that, it uses the English term alone.
- **Technical words** (test, commit, deploy) are written the way developers who speak your language write them. In most languages that is the English word. In Japanese it is テスト, and in Korean it is 테스트.
- **Everyday words** are written in your language, with no English next to them.

It also:

- Matches your dialect and the script you type in. If you write Hindi in Latin letters, it replies in Hindi in Latin letters.
- Fixes word order for right-to-left languages (Arabic, Hebrew, Persian, Urdu), so lines that mix right-to-left and left-to-right text don't come out jumbled.
- Follows your project's rules when they set a language for docs, for example "specs in this repo are written in German".

## Example

You ask, in your language, why new employees can't request annual leave. The first sentence of the reply:

| Your language | Reply |
|---|---|
| Spanish | La causa está en `LeaveService.Submit()`: si el empleado está en `Probation` (período de prueba), lanza una `DomainException` y el `LeaveRequest` (solicitud de vacaciones) se rechaza al instante. |
| Japanese | 原因は `LeaveService.Submit()` です。社員が `Probation`（試用期間）中だと `DomainException` を投げるため、`LeaveRequest`（休暇申請）がすぐ却下され、既存のテストも更新が必要です。 |
| Egyptian Arabic | السبب إن `LeaveService.Submit()` بيرمي `DomainException` لو الموظف في الـ `Probation` (فترة الاختبار)، فالـ `LeaveRequest` (طلب الإجازة) بيترفض على طول. |

The commit message for the same fix stays in English: `fix(leave): route probation annual leave to manager approval`.

## Install

### Claude Code (plugin)

```
/plugin marketplace add edmontdants/codeswitch
/plugin install codeswitch@codeswitch
```

### Any agent that reads skill folders (Claude Code, Codex, Gemini CLI, Copilot CLI)

Copy `skills/code-switching/` into your agent's skills folder:

- `~/.claude/skills/` for Claude Code
- `~/.agents/skills/`, which several agents read

### Make it apply from the first message

Agents load skills only when they need them, so the first reply of a session can come before the skill is loaded. To avoid that, add this line to your always-loaded instructions file (`~/.claude/CLAUDE.md`, `AGENTS.md` or `GEMINI.md`):

```markdown
At the start of every session, load and follow the code-switching skill before writing the first user-facing message.
```

If you installed it as a Claude Code plugin, the skill's name is `codeswitch:code-switching`. Use that name in the line.

The first time you choose your native language, the agent adds a line like this to the same file. You can also add it yourself:

```markdown
User's native language: Egyptian Arabic, Arabic script.
```

If you never want the question, add a fixed reply language instead, for example `Always reply in Spanish.`

## How it was tested

The skill was built test-first. Each version was given to fresh agents running made-up scenarios, first without the change (the baseline) and then with it. Each version ran three times.

| Scenario | Before the change | After |
|---|---|---|
| Explain a bug using domain terms | The same term was paired with a translation up to 4 times; everyday words were also paired with English | Explanation on first mention only, the same name every time |
| Write a handbook section for staff who read Arabic | Written in English, 3 out of 3 runs | Written in Arabic, 3 out of 3 runs |
| Japanese and Korean replies | Loanwords written in Latin letters (test, deploy) | テスト / デプロイ, 테스트 / 배포 |
| Spanish, Hinglish, French, German, Arabic | Passed | Still passing |
| A new session whose first message is in English (v1.1.0) | Picked English or the native language without asking; saved the choice in project memory | Asks once before starting; saves only the native language, user-level (3 out of 3) |

The scenarios are in [`scenarios/`](scenarios/). Run them again after any change to the skill.

## License

[MIT](LICENSE)
