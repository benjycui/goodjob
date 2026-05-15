# Classification Guide

Reference for deciding what to save and where.

## Decision Flow

```
Is this learning derivable from reading the code right now?
  YES → Don't save
  NO  ↓
Is it in git history, docs, or CLAUDE.md already?
  YES → Don't save
  NO  ↓
Is it a generic best practice any developer would know?
  YES → Don't save
  NO  ↓
Is it about the user's preferences or working style?
  YES → feedback memory (built-in) / decision obs (claude-mem)
  NO  ↓
Is it project context not visible in code (why decisions were made, external constraints)?
  YES → project memory (built-in) / discovery obs (claude-mem)
  NO  ↓
Is it a reusable technique that would help in future, unrelated tasks?
  YES → suggest a new personal skill
  NO  ↓
Is it a codebase convention that should be documented?
  YES → suggest CLAUDE.md addition
  NO  ↓
Is it a pointer to an external resource?
  YES → reference memory (built-in) / discovery obs (claude-mem)
  NO  ↓
Is it a mistake or wrong turn worth remembering?
  YES → feedback memory (built-in) / bugfix obs (claude-mem)
  NO  ↓
Is it a tactical insight worth remembering (debugging approach, non-obvious behavior)?
  YES → feedback or project memory depending on scope / discovery obs (claude-mem)
  NO  → Don't save
```

## Good Examples (save these)

**Feedback memory / claude-mem `decision`:**
- "User prefers seeing the full diff before committing, not a summary"
- "User wants errors fixed inline, not extracted to helper functions"
- "User values conservative approaches — one good learning over five mediocre ones"

**Feedback memory / claude-mem `bugfix` (mistakes learned from):**
- "Assumed all skills should be shareable — over-generalized from one case"
- "Tried mocking the database, but prod migration failed — use real DB in integration tests"

**Project memory / claude-mem `discovery`:**
- "Auth rewrite is compliance-driven, not tech-debt — scope decisions should favor compliance over ergonomics"
- "Team freezes merges before mobile release cuts — check with user before PRs near release dates"

**Reference memory / claude-mem `discovery`:**
- "Pipeline bugs tracked in Linear project INGEST"
- "API latency dashboard at grafana.internal/d/api-latency — check when editing request-path code"

**Tactical insight (feedback or project memory / claude-mem `discovery`):**
- "Connection pool state is a non-obvious source of shared mutable state in WebSocket race conditions"
- "When API responses are slow but DB queries are fast, check serialization overhead before assuming network issues"

**Suggest new skill (only when proven across multiple scenarios):**
- A multi-step debugging workflow that proved effective across different codebases
- A code review checklist the user validated across multiple reviews

**Suggest CLAUDE.md addition:**
- Naming conventions not enforced by linters
- Architectural boundaries (e.g., "service X never calls service Y directly")

## Bad Examples (do NOT save these)

- "The project uses React with TypeScript" — visible from package.json
- "Fixed a null pointer in getUserById" — task detail, belongs in git
- "Always handle errors in async functions" — generic best practice
- "The user wants the bug fixed" — obvious from the conversation
- "Tests are in the __tests__ directory" — visible from file structure
- "The API returns JSON" — derivable from code
- "Used console.log for debugging" — not a technique worth saving
- "The function was renamed from X to Y" — in git history
- "User prefers skills as shareable GitHub projects" — over-generalization from one instance; they wanted *this* skill shareable, not all skills
