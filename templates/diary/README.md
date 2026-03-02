# Diary

A CLAUDE.md template that makes Claude keep a monthly journal of work sessions — what was done, what was decided, what was rejected.

## The problem

Claude Code sessions are ephemeral. After a session ends, the conversation is archived but not easily searchable. Over weeks and months, you lose track of:
- When a decision was made and why
- What was tried and abandoned
- The progression of a project across sessions
- Small ideas mentioned in passing

## The solution

A rule in your `CLAUDE.md` that instructs Claude to append entries to a monthly journal file after each significant block of work. No action needed from you — Claude writes silently.

```
## 2026-01-11 14:30 | web/plugins/inventory-sync | v1.2
Worked on plugin v1.2. Improved SQL security with prepare(). Added rollback on sync failure.

## 2026-01-11 16:00 | web/plugins/payment-hooks | webhook architecture
Designed webhook structure. Decided on async queue over synchronous processing.
```

One file per month: `YYYY-MM.journal.md`. Append only. Multiple entries per day is normal.

## Installation

1. Open [template.md](template.md)
2. Replace the placeholders:
   - `JOURNAL_DIR` — where your journal files live (e.g., `~/.claude`, or a project-specific path)
   - `PROJECT_ROOT` — your workspace root for relative paths
   - Exclusions — add directories that should never be journaled (private projects, health data, etc.)
3. Paste the customized block into your `~/.claude/CLAUDE.md` (global) or a project-level `CLAUDE.md`

That's it. Claude will start writing journal entries on its own.

## What it looks like

After a few weeks, your `2026-01.journal.md` might look like:

```markdown
## 2026-01-03 09:15 | web/api | auth refactor
Replaced session-based auth with JWT. Stateless, scales with workers. Kept refresh token rotation.

## 2026-01-03 14:00 | web/api | rate limiting
Added per-user rate limiter. 100 req/min sliding window, Redis counter. Rejected in-memory (doesn't scale across workers).

## 2026-01-05 10:30 | web/frontend | dark mode
Implemented CSS custom properties theme system. User preference stored in localStorage. Respects prefers-color-scheme on first visit.

## 2026-01-05 11:00 | .claude | passing thought
User wants a CLI dashboard for project status. Noted, no action yet.

## 2026-01-08 16:45 | web/api | bug fix
Token expiry was off by 1h — using seconds, API expects ms. One-line fix, big impact.
```

## Excluding directories

Some projects shouldn't be journaled (private, health, confidential). Add exceptions in the template:

```
EXCEPTION: `PROJECT_ROOT/private-stuff` — no journal, no log, no external trace.
```

Claude will skip journaling entirely when working in excluded directories.

## Key design decisions

- **Monthly files, not daily** — one file per month keeps things manageable. Daily files create too much clutter.
- **Append only** — never edit past entries. The journal is a log, not a document.
- **Bash `>>`** — uses append redirection instead of file read/edit to avoid conflicts when multiple Claude sessions run in parallel.
- **System clock timestamps** — Claude runs `date` to get the real time, never invents timestamps.
- **Claude never suggests stopping** — the journal is passive. Claude writes when relevant, never comments on it, never proposes session endings.

## Tips

- **Journal ≠ short-term-memory.** The [short-term-memory](../../skills/short-term-memory/) skill tracks *current* working context (what's happening right now, next step, blockers). The diary tracks *history* (what happened across sessions). They're complementary.
- **Grep is your friend.** `grep -r "rate limit" ~/.claude/2026-*.journal.md` finds every session where rate limiting was discussed.
- **Keep entries natural.** 2-10 lines, plain language. Not a commit message, not a standup report. Write like you're telling a colleague what you did today.
- **One line is fine.** A quick question that took 30 seconds still gets a one-line entry. Better noisy than incomplete.
