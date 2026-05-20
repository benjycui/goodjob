---
name: goodjob
description: "Use when users express appreciation for the agent's work - phrases like 'good job', 'nice work', 'thanks that was perfect', 'well done', 'exactly what I needed', or similar positive feedback about completed work."
---

# Good Job — Learn from What Worked

When a user appreciates your work, review the conversation and save non-obvious learnings.

## Step 1: Assess Signal Quality

**Substantive** — references specific work or approach:
- "That debugging approach was perfect"
- "Good job, I didn't know about that pattern"

**Borderline** — mild appreciation with vague reference:
- "thanks, that was helpful"
- "nice, looks good"

If borderline: acknowledge and do a **quick scan** only. If nothing non-obvious jumps out within a few seconds of review, stop. Don't dig.

**Casual** — polite acknowledgment, no specifics:
- "thanks"
- "ok good"

If casual: acknowledge briefly and **stop**. Do not force learnings.

## Step 2: Review Conversation

Scan the recent work for **non-obvious** learnings. Look for:

- What approach or technique the user valued, and why
- Decisions that worked and the reasoning behind them
- Non-obvious discoveries, debugging insights, or workarounds
- Mistakes, wrong turns, or dead ends — and what was learned
- User corrections or pushback that revealed preferences
- External resources that proved useful

**If nothing went poorly, skip that part.** Don't manufacture lessons.

**Hard filter — do NOT save:**
- Anything derivable from reading the current code or git history
- Anything in existing docs or CLAUDE.md
- Generic best practices ("use TypeScript", "write tests")
- Specific task details (what files changed, what the bug was)
- **Over-generalizations** — a preference for *this* task doesn't mean a preference for *every* task. Save the specific observation, not a broad policy.

**Over-generalization test:** Before saving, ask: "Would this still be true if the user gave me a completely different task tomorrow?" If the answer is "maybe not," scope it down or skip it.

**Cap: save at most 2-3 learnings per trigger.** If you found more, keep only the most non-obvious. Quality over quantity — one sharp insight beats five mediocre ones.

"Nothing non-obvious to capture" is a valid and common outcome.

See [classification-guide.md](classification-guide.md) for detailed examples.

## Step 3: Classify and Persist

### Choose storage backend FIRST

Check if claude-mem MCP tools are available (look for `claude-mem` in the available skills/tools list). **If claude-mem is installed, use it as the PRIMARY storage — do NOT fall back to built-in memory.** Built-in memory is only for when claude-mem is not available.

### Then classify each learning:

| Learning Type | claude-mem (preferred) | Built-in memory (fallback) |
|---|---|---|
| User preference or working style | `decision` observation | `feedback` memory |
| Project context not in code | `discovery` observation | `project` memory |
| External resource pointer | `discovery` observation | `reference` memory |
| Mistake or wrong turn learned from | `bugfix` observation | `feedback` memory |
| Tactical insight worth remembering | `discovery` observation | `feedback` or `project` memory |
| Reusable technique (proven across multiple scenarios) | Suggest creating a new personal skill | Suggest creating a new personal skill |
| Codebase convention not yet documented | Suggest CLAUDE.md addition | Suggest CLAUDE.md addition |

**Skill suggestion threshold:** Only suggest a new skill when a technique has proven effective across multiple distinct scenarios. A single good insight is a memory, not a skill.

## Step 4: Report

One line per learning: what it is and where it was saved.

If nothing non-obvious was found: "Nothing non-obvious to capture this time — the good work speaks for itself."

Keep it brief. The user said "good job", not "give me a report."
