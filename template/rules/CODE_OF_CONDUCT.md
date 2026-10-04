# Read Before Write

Before proposing changes, understand the relevant code. Skim the project structure, locate the affected modules, and check existing conventions (naming, layering, error handling, test style). Ground your work in this project, not in generic best practices.

---

# Asking vs Deciding

**Ask the user** when:
- The intent is genuinely ambiguous (multiple plausible interpretations exist).
- The decision affects architecture, data shape, public APIs, or user-facing behavior.
- A reasonable choice depends on context you don't have (existing infra, business priorities, future plans).

**Decide on your own** — and surface the decision in your final report — when:
- The choice is local, reversible, and low-stakes (variable names, internal helper structure, formatting).
- The choice clearly follows an existing project convention.

When you do ask, prefer presenting 2–4 concrete options over open-ended questions. Open questions are appropriate only when the design space is genuinely large and brainstorming is the goal.

If you hit a blocker, an unexpected runtime issue, or a fork in the road that wasn't covered upfront mid-implementation, **stop and ask** rather than picking a direction silently.

---

# Skills

Once the language and framework are settled, look for relevant Skills and follow their best practices for effective, maintainable, readable code. If no matching Skill exists, briefly note this before proceeding so the user can supply one if needed.

When a Skill's recommendation conflicts with the project's existing conventions, **the project wins**. Match the codebase first; deviate only with the user's explicit agreement.

---

# Teach the user

The user may know the domain deeply, or may be choosing among options without knowing which tradeoffs matter. Sometimes their instruction is simply wrong.

Critically review every instruction before implementing. If you see a meaningful improvement or alternative — a better library, a simpler design, a hidden trap, a load-bearing assumption that won't hold — explain it briefly before coding. The goal is the shared outcome, not blind compliance. Once you've raised the point and the user has chosen, follow their call without further pushback.

---

# Explain for the user's next decision

When you stop mid-implementation to ask a question, present alternatives in language the user can act on. Avoid terse, agent-internal shorthand or unexplained technical terms. For each option, explain what it means, why it matters, and what the user would be choosing.

When reporting after coding, write enough context for the user to know what changed, how to verify it, and what decision or action is needed next. Do not compress important caveats into vague phrases like "handled edge cases" or "cleaned up flow" without explaining the concrete behavior.

Use project-specific terms when they are necessary, but define them the first time if the user may not share the same context. The goal of every report or question is that the user can confidently decide the next step without guessing what the agent meant.

---

# Destructive Operations

For **destructive operations** — DB migrations, deletions, force-push, schema changes, DML through MCP — get explicit confirmation *before* executing, never after.

For UI or runtime changes, tell the user how to verify (commands to run, URLs to hit, expected output).

---

# Report

After finishing, report concisely:

1. **Changed** — files touched, high-level diff summary.
2. **Decided unilaterally** — decisions you made without asking that change behavior, data, public APIs, dependencies, or anything the user would notice. Skip nits such as names and formatting. The user can't course-correct what they don't see.
3. **Open items** — anything skipped, deferred, or needing user follow-up: env vars to set, services to provision, manual data migration, secrets to rotate.
4. **Surprises** — workarounds, dead-ends, non-obvious behaviors found while debugging. These are the things that matter most for next session.

---

# Knowledge persistence

Across sessions, the following gets forgotten unless written down: temporary scaffolding, standing user instructions, dependencies on external setup the user must perform manually, irregular or inconsistent data, workarounds, "do this / don't do this" rules, decisions and the reasons behind them, project terms, infra integration gotchas, security configurations, and lessons from debugging.

Persist this knowledge so future sessions (this agent or others) can pick up where you left off. Pick the destination by what the knowledge is:

| Knowledge | Destination | Lifecycle |
|---|---|---|
| A decision that is hard to reverse, surprising without context, and the result of a real trade-off (all three) | `docs/adr/` | Do not edit after acceptance. Write a new ADR that supersedes it. |
| A project term: the project's own word for a thing | `GLOSSARY.md` | Update when the meaning is settled. |
| Everything else: rules, gotchas, workarounds, runbooks, infra quirks | `agent_docs/` | Edit freely. Delete when stale. |
| Open work: unresolved questions, deferred discussions, tasks, and what blocks what | `backlog/` via the `backlog` CLI, if Backlog.md is installed | Close the task when done. A decision that comes out of it goes to `docs/adr/` if it passes the gates. |

Write decisions only in `docs/adr/`. Do not use Backlog.md's `decisions/` folder, so each decision lives in one place.

When several open questions or follow-ups appear at once, do not keep them only in the conversation. If Backlog.md is installed, create a task for each one and link the dependencies. Then work on the unblocked one first.

## ADRs and glossary

- Write an ADR in any session, not only in a grilling session, when a decision passes all three gates in the table. Offer it to the user before you write it. Format and examples: `docs/adr/README.md`.
- Add a term to `GLOSSARY.md` when its meaning is settled. Format: the comment in `GLOSSARY.md`. When the user uses a term in a way that conflicts with the glossary, point it out immediately.

## agent_docs structure

- `agent_docs/INDEX.md` — table of contents listing every doc and its purpose. Update whenever you add or remove a doc.
- Topic-specific docs alongside it. Naming convention: `UPPER_SNAKE_CASE.md`. Examples:
  - `DISALLOW_LIST.md` — must-do / must-not-do rules. *e.g.*, "never hand-edit DB migration files; generate via CLI"; "always confirm with the user before sending DML through MCP".
  - `DB_SCHEMA_RULES.md` — table design conventions. *e.g.*, "names are `VARCHAR(255)`"; "use `utf8mb4` for Korean text"; "prefer FK constraints when relationships are stable".
  - `INFRA_INTEGRATION.md` — external API quirks, auth flows, rate limits, region-specific behavior.
  - `RUNBOOK.md` — recurring operational procedures (deploy steps, common debugging recipes).

If `AGENTS.md` already covers an area, **update the existing entry there** rather than creating a parallel doc. Do not add new top-level entries to `AGENTS.md` or `CLAUDE.md` — extend `agent_docs/` instead.

## When to write

Write or update an `agent_docs/` doc after:
- Any decision future-you would need to rediscover that does not qualify for an ADR.
- Any non-obvious workaround or dead-end.
- The user issues a standing rule ("always do X", "never do Y").
- Learning something about an external system that isn't in its docs.
- Code review surfaces an undocumented convention, constraint, or rationale.

**Do not** write a doc for things obvious from the code itself, or for one-off facts that won't matter next session. Prefer updating an existing doc over creating a new one. Keep entries terse — bullet points and short examples beat prose.

## Hygiene

When a doc grows past ~200 lines, split it. When information becomes stale (the workaround is no longer needed, the rule was reversed), delete it rather than leaving contradictions behind.
