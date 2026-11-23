---
name: code-switching
description: Use at the start of every session before the first user-facing message, and whenever choosing the language of a reply, file, commit, message or session title - especially when the user writes in a language other than English (native script, romanized, or mixed with English technical terms), or sends a short English prompt or pasted English content in a session whose chat language is not English.
---

# Code-Switching

## Overview
Talk to the user in their native language so they read fast. Keep technical
and domain terms in English, spelled exactly as the code and the domain name
them. Keep engineering artifacts in English. Concise, never lossy. Works for
any language; the examples below are illustrations, not the target.

## Step 0: Chat language for this session
Keep two facts apart:
- **Native language** describes the user, so it is global. It is recorded once,
  as the single line `User's native language: <language, dialect, script>`, in
  the user-level always-loaded instructions file (`~/.claude/CLAUDE.md`,
  `~/.codex/AGENTS.md`, `~/.gemini/GEMINI.md`). If that file can't be written,
  use the agent's user-level memory. Never record it in project memory.
- **Chat language** is chosen per session: the native language or English.
  It is never recorded.

At the user's first message of a session, apply the first rule that matches:
1. The user names a reply language, in the message or in always-loaded instructions that fix one: use it.
2. The user's own words are in another language, even one short phrase: that is the chat language, so don't ask. Match its dialect and script (romanized Hindi gets romanized Hindi). If no native language is recorded, record this one.
3. No one can answer (a scheduled or headless run): use the native language if recorded, else English.
4. Otherwise, ask once before starting the work, with the agent's question tool if it has one:
   - native language recorded: "English, or <native language in its own script>?";
   - not recorded: "English, or your native language? If yours, which one?" When the user names one, record it as the native language and say where in one line.

Then do the work in the chosen language. Keep it for the whole session without
asking again: a short English prompt, pasted English content or an English
subagent report is not a switch. Switch only when the user asks, or writes full
messages in the other language. English chat means no translations or glosses.

The user's own words exclude pasted text, logs, errors, code, file names and
labels. A name or email address is never evidence.

## Step 1: Classify each output by who reads it

| Class | Examples | Language |
|---|---|---|
| **User Communication** (the user reads it now) | Replies, progress lines, questions and their options, summaries, relayed subagent reports, wrap-up recaps | Chat language, with the term rules in Step 2 |
| **Engineering Artifact** (engineers, tools or agents read it) | Code, comments, tests, configs, specs, plans, BRDs, analyses and reports about the work (even for business stakeholders), commits, PRs, review comments, agent instructions, subagent prompts | English |
| **Audience Content** (other people read it as the product or as a message) | UI strings, locale files, published end-user or employee content (handbooks, policies, help pages), emails or chat messages sent for the user | The audience's language: the file's locale (`fr.json` is French), else what the user said about the readers, else existing content in the same place; if still unknown, ask once |
| **Raw Tool Output** | Command output, logs, stack traces, errors, diffs | Exact, untouched; explain it in the chat language |
| **Session Metadata** | Session titles, branch names, task-list items, tool-call descriptions | Concise English |

A language the user sets for an output, or the project's always-loaded
instructions set for a kind of output (for example "specs in this repo are
in German"), wins over these defaults.

## Step 2: Terms in User Communication

| Kind of word | How to write it |
|---|---|
| Domain and code terms: entities, models, enums, statuses, roles, rules, modules, the Ubiquitous Language | English, exactly as the code or domain spells it; identifiers in backticks. On the **first mention only**, add a short gloss in the chat language if the meaning is not obvious. Every later mention: the English term alone. |
| Technical terms developers say in English (test, commit, deploy, endpoint) | The form developers who speak the chat language write: usually the English word; in languages that write loanwords in their own script, that form (Japanese テスト, コミット). Never glossed. |
| Everyday words (employee, policy, formatting) | Chat language only, never paired with English |

- One name per concept for the whole session. Reuse the first English term; never switch to a second translation or a synonym. A project glossary, if one exists, wins.
- Use the English name when you mean the domain concept or code element; use plain chat-language words when you just describe people or things.
- Attach the chat language's grammar to English terms the way its developers do: articles, prefixes, particles, case endings.
- Cut preambles, restated questions and filler. Keep risks, warnings, decisions, trade-offs and next steps.

## Step 3: Layout
- Put commands, paths and multi-line code in fenced blocks.
- Keep a term and its gloss on one line.
- Right-to-left scripts (Arabic, Hebrew, Persian, Urdu): start every paragraph, bullet, heading and table cell with a chat-language word, not an English term or a number, so mixed-direction lines don't render scrambled.

## Red flags: you are drifting
| You are about to | Do instead |
|---|---|
| Gloss a term you already glossed earlier | English term alone |
| Pair an everyday word with its English | Chat-language word alone |
| Name a concept differently from earlier in the session | Reuse the first name |
| Start a session's work in English, or in the recorded native language, when the first message was English and nothing chose a language | Ask first (Step 0, rule 4) |
| Save the session's chat choice, or save the native language to project memory | Record only the native language, user-level |
| Switch to English mid-session because the prompt, the paste or a subagent report was English | Keep the session's chat language |
| Write user-facing product text or staff content in English because "files are English" | Audience Content: the readers' language |
| Translate a spec, BRD, commit or code comment into the chat language without an instruction to | Engineering Artifact: English |

## Example
The same first sentence of a bug explanation, in three chat languages:

| Chat language | Reply |
|---|---|
| Spanish | La causa está en `LeaveService.Submit()`: si el empleado está en `Probation` (período de prueba), lanza una `DomainException` y el `LeaveRequest` (solicitud de vacaciones) se rechaza al instante. |
| Japanese | 原因は `LeaveService.Submit()` です。社員が `Probation`（試用期間）中だと `DomainException` を投げるため、`LeaveRequest`（休暇申請）がすぐ却下され、既存のテストも更新が必要です。 |
| Egyptian Arabic | السبب إن `LeaveService.Submit()` بيرمي `DomainException` لو الموظف في الـ `Probation` (فترة الاختبار)، فالـ `LeaveRequest` (طلب الإجازة) بيترفض على طول. |

Later sentences use `Probation` and `LeaveRequest` without the gloss.
Commit: `fix(leave): route probation annual leave to manager approval`.
Title: `Route probation leave to manager approval`.

## Scope
Applies to user-facing output only; never changes implementation or
engineering quality. Pair it with one line in the always-loaded instructions
file (`CLAUDE.md`, `AGENTS.md`, `GEMINI.md`) so it applies from the first
message.
