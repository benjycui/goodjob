# goodjob

A Claude Code skill that learns from appreciation. When you say "good job" or similar, it reviews the conversation and saves non-obvious learnings to the right place.

## What It Does

When you appreciate your AI agent's work, `goodjob` triggers a review of the conversation and:

1. **Filters** — ignores casual "thanks" and only acts on substantive appreciation
2. **Reviews** — scans for non-obvious learnings (not things derivable from code or git)
3. **Classifies** — determines where each learning belongs (supports both built-in memory and [claude-mem](https://github.com/thedotmack/claude-mem)):
   - User preferences → feedback memory / claude-mem `decision`
   - Project context → project memory / claude-mem `discovery`
   - Mistakes learned from → feedback memory / claude-mem `bugfix`
   - Tactical insights → feedback or project memory / claude-mem `discovery`
   - Reusable techniques (proven across multiple scenarios) → suggests new personal skill
   - Codebase conventions → suggests CLAUDE.md addition
   - External resources → reference memory / claude-mem `discovery`
4. **Reports** — tells you what was saved and where

## Installation

### As a plugin (recommended)

```bash
claude plugin add github:benjycui/goodjob
```

### As a personal skill (manual)

```bash
git clone https://github.com/benjycui/goodjob.git /tmp/goodjob
mkdir -p ~/.claude/skills
cp -r /tmp/goodjob/skills/goodjob ~/.claude/skills/goodjob
rm -rf /tmp/goodjob
```

## Usage

Just work with Claude Code as usual. When the agent does something you value, tell it:

- "Good job, that approach was exactly right"
- "Nice work, I didn't know about that pattern"
- "Thanks, that was perfect — the way you debugged that was great"

The skill triggers automatically and saves relevant learnings.

Casual "thanks" or "ok good" won't trigger a full review — the skill is conservative by design.

## Requirements

- **Claude Code** — the skill uses Claude Code's built-in memory system

### Optional (enhances but not required)

- **[claude-mem](https://github.com/thedotmack/claude-mem)** — provides searchable, cross-session memory stored in SQLite. When installed, goodjob persists learnings as claude-mem observations (searchable across all projects and sessions) instead of plain markdown files.
- **[superpowers](https://github.com/obra/superpowers)** — enables auto-triggering via the "even 1% chance" skill matching rule. Without it, invoke manually with `/goodjob`.

## Where Learnings Are Saved

| Learning Type | Built-in Memory | claude-mem |
|---|---|---|
| User preference / working style | `feedback` memory | `decision` observation |
| Project context not in code | `project` memory | `discovery` observation |
| External resource reference | `reference` memory | `discovery` observation |
| Mistakes learned from | `feedback` memory | `bugfix` observation |
| Tactical insight | `feedback` or `project` memory | `discovery` observation |
| Reusable technique (proven across multiple scenarios) | New skill via `/writing-skills` | New skill via `/writing-skills` |
| Codebase convention | CLAUDE.md addition | CLAUDE.md addition |

## What It Won't Save

The skill is intentionally conservative. It skips:

- Anything visible in the code or git history
- Generic best practices
- Specific task details (those belong in commits)
- Obvious information from the conversation

"Nothing to capture this time" is a valid outcome.

## License

MIT
