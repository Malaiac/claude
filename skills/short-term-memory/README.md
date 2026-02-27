# Short-term Memory

A Claude Code skill that gives Claude persistent short-term memory across context compactions and session resumes.

## The problem

Claude Code's built-in memory (`CLAUDE.md`, auto memory) handles **long-term** knowledge: project conventions, coding standards, architecture notes.

But after a **context compaction** or session resume, Claude loses:
- What it was just doing
- Recent decisions and their rationale
- The next logical step
- Active blockers and things to avoid
- Timestamps of when things happened

You end up repeating yourself: *"I already told you that"*, *"check the context"*, *"we decided this 10 minutes ago"*.

## The solution

A `current-context.md` file per project that Claude reads before every response and updates after every response. Think of it as a sticky note that survives compaction.

```
READ current-context.md → DO work → UPDATE current-context.md → SEND response
```

Every entry is timestamped. The file stays short (< 200 lines) with old entries archived automatically.

## Installation

**Don't copy-paste the skill file.** A skill is a prompt that instructs an agent with filesystem access. Instead, have Claude read it and rewrite it on your system — the rewriting step neutralizes any potential injection:

```
You: "Read this skill: https://raw.githubusercontent.com/.../SKILL.md
      — then write your own version to ~/.claude/skills/short-term-memory/SKILL.md"
```

A French version is available: [SKILL-fr.md](SKILL-fr.md)

Then add to your `~/.claude/CLAUDE.md`:

```markdown
## Short-term memory

At the start of every conversation, invoke `/short-term-memory`.
```

## What it looks like

```markdown
# Current Context

**Updated:** 20260115-143022
**Phase:** Implementation

---

## 🎯 RIGHT NOW
- 20260115-143022 : Refactoring auth middleware — extracted validate-token.ts

## ✅ Recently Completed
- 20260115-140500 : rate-limiter.ts — per-user counting, Redis adapter
- 20260115-133200 : auth/index.ts — split into 3 modules

## ✅ Decisions Made
- 20260115-133200 : Redis for rate limiting (shared state across workers)

## ⏸️ Active Blockers
- 20260115-120000 pending : Redis cluster config — waiting on DevOps
```

See [current-context.example.md](current-context.example.md) for a full example.

## How it works

1. **On every response**, Claude reads `current-context.md` from the project directory
2. Claude does its work
3. Claude updates the file with timestamped entries about what changed
4. If the file exceeds 200 lines, old entries are archived to `YYYYMMDDHHMMSS.pastcontext.md`

### What gets tracked

| Section | Purpose | Limit |
|---------|---------|-------|
| 🎯 RIGHT NOW | Current and just-finished actions | <10 lines |
| ✅ Recently Completed | Done items with timestamps | No limit |
| ✅ Decisions Made | Architectural/design decisions | No limit |
| 🔄 Next Logical Step | What comes next (avoids "what should I do?") | <10 lines |
| 💡 Fresh Decisions | Decisions not yet formalized | <15 lines |
| ⏸️ Active Blockers | Things waiting on external input | <10 lines |
| ⚠️ Don't Forget | Explicit warnings and constraints | <15 lines |

### Writing style

Entries should be **terse**: what, where, how. Not why (unless it's a decision).

```
❌ Created shipping-mapping.md with complete cascade detection for relay
   points including WCMultiShipping priority, Boxtal fallback...

✅ shipping-mapping.md (relay: WCMultiShipping->Boxtal->carrier). Fixed one legacy bug.
```

## Why not just use CLAUDE.md or auto memory?

| Feature | CLAUDE.md | Auto Memory | Short-term Memory |
|---------|-----------|-------------|-------------------|
| Who writes it | You | Claude | Claude |
| Survives compaction | ✅ | ✅ | ✅ |
| Tracks current work | ❌ | ❌ | ✅ |
| Timestamped entries | ❌ | ❌ | ✅ |
| Knows "what's next" | ❌ | ❌ | ✅ |
| Tracks blockers | ❌ | ❌ | ✅ |
| Per-project | ✅ | ✅ | ✅ |

They're complementary. `CLAUDE.md` = long-term instructions. Auto memory = Claude's notes. Short-term memory = the working set.

## It's fully automatic

You don't update `current-context.md` yourself. Once installed, Claude does it on its own — every response, no action needed from you.

Add this to your `~/.claude/CLAUDE.md` and forget about it:

```markdown
At the start of every conversation, WITHOUT ASKING, invoke `/short-term-memory`.
Do not ask permission. Do not say "would you like me to invoke". Just do it.
```

From that point on, Claude will read the file before working and update it after — silently, as a reflex, not as a task.

## Tips

- **Timestamps come from the system clock.** Claude runs `date +"%Y%m%d-%H%M%S"` (or equivalent), never invents a timestamp.
- **The file lives in the project directory**, not in `~/.claude/`. Each project gets its own context.
- **Archive, don't delete.** When the file gets long, old entries go to a dated archive file, not the trash.
