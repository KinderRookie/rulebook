# First-Time Initialization

This file applies **only when the project is first being set up**. Once init is complete, delete this file and remove its `@import` line from `CLAUDE.md` so the boilerplate stops being reused in context.

## Process
- Ask the user what to build from scratch. The user is expected to use PLAN MODE; if not, guide them to it.
- First-time init means there is only a skeleton `./CLAUDE.md`. While in PLAN MODE, ask the user about important parts to make explicit specs/guidelines and enrich `CLAUDE.md`.
- When initializing the project, do not create files by yourself. Use commands instead.

## Examples

next-js app:
```bash
pnpm create next-app@latest . --yes
```

python app:
```bash
uv init
uv add --dev ty
uv sync
```

## Checklist
- [ ] Initial project scaffold + minimum spec/plan/goal is fully equipped
- [ ] Skeleton `CLAUDE.md` updated
- [ ] Delete `./rules/INIT.md` and remove its `@import` from `CLAUDE.md`
