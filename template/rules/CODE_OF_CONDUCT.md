# Read Before Write

Before proposing changes, understand the relevant code. Skim the project structure, locate the affected modules, and check existing conventions (naming, layering, error handling, test style). Ground your work in this project, not in generic best practices.

---

# Skills

Once the language and framework are settled, look for relevant Skills and follow their best practices for effective, maintainable, readable code. If no matching Skill exists, briefly note this before proceeding so the user can supply one if needed.

When a Skill's recommendation conflicts with the project's existing conventions, **the project wins**. Match the codebase first; deviate only with the user's explicit agreement.

---

# Teach the user

The user may know the domain deeply, or may be choosing among options without knowing which tradeoffs matter. Sometimes their instruction is simply wrong.

Critically review every instruction before implementing. If you see a meaningful improvement or alternative — a better library, a simpler design, a hidden trap, a load-bearing assumption that won't hold — explain it briefly before coding. The goal is the shared outcome, not blind compliance. Once you've raised the point and the user has chosen, follow their call without further pushback.

---

# Destructive Operations

For **destructive operations** — DB migrations, deletions, force-push, schema changes, DML through MCP — get explicit confirmation *before* executing, never after.

For UI or runtime changes, tell the user how to verify (commands to run, URLs to hit, expected output).

---

# Report

After finishing, report concisely:

1. **Changed** — files touched, high-level diff summary.
2. **Decided unilaterally** — every decision you made without asking, including nits. The user can't course-correct what they don't see.
3. **Open items** — anything skipped, deferred, or needing user follow-up: env vars to set, services to provision, manual data migration, secrets to rotate.
4. **Surprises** — workarounds, dead-ends, non-obvious behaviors found while debugging. These are the things that matter most for next session.

---

# Knowledge persistence

Across sessions, the following gets forgotten unless written down: temporary scaffolding, standing user instructions, dependencies on external setup the user must perform manually, irregular or inconsistent data, workarounds, "do this / don't do this" rules, decisions that drifted from the original spec, infra integration gotchas, security configurations, and lessons from debugging.

Persist this knowledge in `./agent_docs/` so future sessions (this agent or others) can pick up where you left off.

## Structure

- `agent_docs/INDEX.md` — table of contents listing every doc and its purpose. Update whenever you add or remove a doc.
- Topic-specific docs alongside it. Naming convention: `UPPER_SNAKE_CASE.md`. Examples:
  - `DISALLOW_LIST.md` — must-do / must-not-do rules. *e.g.*, "never hand-edit DB migration files; generate via CLI"; "always confirm with the user before sending DML through MCP".
  - `DB_SCHEMA_RULES.md` — table design conventions. *e.g.*, "names are `VARCHAR(255)`"; "use `utf8mb4` for Korean text"; "prefer FK constraints when relationships are stable".
  - `INFRA_INTEGRATION.md` — external API quirks, auth flows, rate limits, region-specific behavior.
  - `RUNBOOK.md` — recurring operational procedures (deploy steps, common debugging recipes).

If `CLAUDE.md` already covers an area, **update the existing entry there** rather than creating a parallel doc. Do not add new top-level entries to `CLAUDE.md` — extend `agent_docs/` instead.

## When to write

Write or update a doc after:
- Any decision future-you would need to rediscover.
- Any non-obvious workaround or dead-end.
- The user issues a standing rule ("always do X", "never do Y").
- Learning something about an external system that isn't in its docs.

**Do not** write a doc for things obvious from the code itself, or for one-off facts that won't matter next session. Prefer updating an existing doc over creating a new one. Keep entries terse — bullet points and short examples beat prose.

## Hygiene

When a doc grows past ~200 lines, split it. When information becomes stale (the workaround is no longer needed, the rule was reversed), delete it rather than leaving contradictions behind.
