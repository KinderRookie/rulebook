# Architecture Decision Records

Each file records one decision and the reason for it. Rules: `rules/CODE_OF_CONDUCT.md` → "ADRs".

## When to write one

Write an ADR only when all three are true:
1. **Hard to reverse.** Changing your mind later has a real cost.
2. **Surprising without context.** A future reader will ask "why did they do it this way?"
3. **A real trade-off.** There were real alternatives, and one was picked for specific reasons.

If one is false, do not write an ADR. Put the knowledge in `agent_docs/` or leave it in the conversation.

Decisions that usually qualify: architectural shape, integration patterns, technology with lock-in, boundary and scope decisions, deliberate deviations from the obvious path, constraints not visible in the code, and non-obvious rejected alternatives.

## File name

`NNNN-slug.md`. Find the highest number in this directory and add one. Example: `0001-postgres-for-write-model.md`.

## Format

```md
# {Short title of the decision}

{1-3 sentences: the context, the decision, and why.}
```

An ADR can be one paragraph. Add these sections only when they add real value:
- **Status**: `proposed | accepted | deprecated | superseded by ADR-NNNN`.
- **Considered Options**: only when a rejected option is worth remembering.
- **Consequences**: only when a downstream effect is not obvious.

Do not edit an accepted ADR. To change a decision, write a new ADR and mark the old one `superseded by ADR-NNNN`.

Format adapted from [mattpocock/skills `domain-modeling`](https://github.com/mattpocock/skills/tree/main/skills/engineering/domain-modeling) (MIT).
