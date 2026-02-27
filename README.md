# Claude Skills

A collection of [Claude Code skills](https://code.claude.com/docs/en/skills) to enhance Claude's capabilities.

## Skills

| Skill | Description |
|-------|-------------|
| [short-term-memory](skills/short-term-memory/) | Persistent working context across compactions and session resumes |

## Installation

**Don't copy-paste skill files directly.** A skill is a prompt that instructs an agent with filesystem access. Pasting a prompt from the internet is a potential injection vector.

Instead, have Claude read the skill and rewrite it on your system:

```
You: "Read this skill file: https://raw.githubusercontent.com/.../SKILL.md
      — then write your own version to ~/.claude/skills/short-term-memory/SKILL.md"
```

The rewriting step neutralizes any embedded injection. Claude understands the intent and produces a clean version adapted to your setup.

Then reference it in your `~/.claude/CLAUDE.md`:

```markdown
At the start of every conversation, invoke `/short-term-memory`.
```

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

Each skill lives in `skills/<skill-name>/` and must contain:
- `SKILL.md` — the skill file Claude loads (compact, operational)
- `README.md` — human-readable documentation (why, how, examples)

## License

MIT
