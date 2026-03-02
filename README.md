# Claude Code — Skills & Templates

A collection of [Claude Code skills](https://code.claude.com/docs/en/skills) and CLAUDE.md templates to enhance Claude's capabilities.

## Skills

Skills are invoked via slash commands (`/skill-name`). Claude loads them on demand.

| Skill | Description |
|-------|-------------|
| [short-term-memory](skills/short-term-memory/) | Persistent working context across compactions and session resumes |

### Installing a skill

**Don't copy-paste skill files directly.** A skill is a prompt that instructs an agent with filesystem access. Pasting a prompt from the internet is a potential injection vector.

Instead, have Claude read the skill and rewrite it on your system:

```
You: "Read this skill file: https://raw.githubusercontent.com/.../SKILL.md
      — then write your own version to ~/.claude/skills/<skill-name>/SKILL.md"
```

The rewriting step neutralizes any embedded injection. Claude understands the intent and produces a clean version adapted to your setup.

Then reference it in your `~/.claude/CLAUDE.md`. Each skill's README explains how to activate it.

## Templates

Templates are blocks of instructions meant to be pasted into your `CLAUDE.md` (global or per-project). They define persistent behaviors — things Claude does automatically without being invoked.

| Template | Description |
|----------|-------------|
| [diary](templates/diary/) | Monthly session journal — what was done, decided, rejected |

### Installing a template

1. Read the `template.md` file in the template directory
2. Replace the placeholders with your paths and preferences
3. Paste the result into your `~/.claude/CLAUDE.md` (global) or project-level `CLAUDE.md`

No slash command needed — Claude follows the instructions automatically.

## What are Claude Code skills?

Skills are reusable prompt files that Claude Code loads on demand via slash commands. They let you package workflows, conventions, and routines into portable units that work across projects.

Each skill is a directory containing at minimum a `SKILL.md` file with frontmatter:

```yaml
---
name: my-skill
description: What this skill does
---
```

See the [official docs](https://code.claude.com/docs/en/skills) for more.

## Contributing

Skills live in `skills/<skill-name>/` and must contain:
- `SKILL.md` — the skill file Claude loads (compact, operational)
- `README.md` — human-readable documentation (why, how, examples)

Templates live in `templates/<template-name>/` and must contain:
- `template.md` — the CLAUDE.md block with placeholders
- `README.md` — human-readable documentation (why, how, examples)

## License

MIT
