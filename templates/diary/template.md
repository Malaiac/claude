## Diary

EVERY session, you MUST append an entry to the monthly journal.

### When to write

- At each significant milestone (bug fixed, feature done, refactor complete)
- At each a-ha moment or pivotal decision
- When the user signals end of session ("good night", "we're done", "I'm off")

**"thanks", "ok", "done" = acknowledgment, NOT end of session.** Only explicit departure signals count.

**A little noise is better than missed episodes.** Multiple entries per session = normal.

### File

`JOURNAL_DIR/YYYY-MM.journal.md` (replace YYYY-MM with current year-month).

Create the file if it doesn't exist.

### Exclusions

Some directories may be excluded from journaling. Define them here:

```
EXCEPTION: `PROJECT_ROOT/private-project` — no journal, no log, no external trace.
```

### Format

```
## YYYY-MM-DD HH:MM | [project directory] | [free context]
[Natural summary of what was done, discussed, decided. 2 to 10 lines depending on substance. Favor useful content over brevity, but don't drown in unnecessary technical detail.]
```

Project directory = relative path from your root (e.g., `web/plugins/my-plugin`, `projects/acme-app`). Free context = optional short description.

### Examples

```
## 2026-01-11 14:30 | web/plugins/inventory-sync | v1.2
Worked on plugin v1.2. Improved SQL query security with prepare(). Added rollback on sync failure. Not deployed yet, waiting for client validation Monday.

## 2026-01-11 16:00 | web/plugins/payment-hooks | webhook architecture
Designed webhook structure. Decided on async queue over synchronous processing. To explore: Redis vs job scheduler.

## 2026-01-11 18:00 | .claude | passing thought
User mentioned wanting a tagging system for projects. Noted for later, no immediate action.
```

### Rules

- **ALWAYS write**, even for a short session or a simple question (one line is enough)
- **Append only** — never edit previous entries unless explicitly asked
- **Create the month's file** if it doesn't exist
- **Timestamps from system clock** — never invent a timestamp
- If content volume conflicts with journal brevity, write a detailed file in the project and keep the journal entry concise

### Concurrency

Use bash `>>` (append) rather than read/edit to avoid conflicts between simultaneous sessions:

```bash
printf '## 2026-01-19 13:30 | project\nSession content...\n\n' >> "JOURNAL_DIR/2026-01.journal.md"
```

