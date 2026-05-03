# rulebook

명확하고 유지보수 가능한 agent-paired 코딩을 위한 프로젝트 템플릿 입니다. 

A meta-template for coding agents (Claude Code, Cursor, Codex, etc.). Drop the skeleton into a fresh project and your agent inherits a consistent set of working rules, code conventions, and a knowledge-persistence convention — without you re-pasting them every time.

## Install

In your project root:

```bash
npx degit --force snagreakim/rulebook/template .
git init   # if you haven't already
```

`--force` is needed if the directory already has files (e.g. `.git/` from `git init`, or scaffolding from `pnpm create next-app`). degit copies only the files present in the template, so it won't touch your existing source files unless their paths collide with skeleton files (`CLAUDE.md`, `rules/*`, `agent_docs/*`).

No node? Use curl:

```bash
curl -L https://github.com/snagreakim/rulebook/archive/main.tar.gz \
  | tar -xz --strip-components=2 rulebook-main/template
```

### Recommended order

1. `mkdir myproject && cd myproject`
2. `npx degit --force snagreakim/rulebook/template .`
3. `git init`
4. Open the project in your agent — it follows `rules/INIT.md` to scaffold the actual app (e.g. `pnpm create next-app .`) and fill in `CLAUDE.md`.

## What you get

```
CLAUDE.md            # Project entry — placeholders for stack/structure, @imports the rules
rules/
  INIT.md            # First-time bootstrap guide (delete after init)
  CODE_OF_CONDUCT.md # How the agent works WITH you (clarify, teach, report, persist)
  CODING_CONVENTION.md          # How the agent writes code (think, simplify, surgical, goal-driven)
  CODING_CONVENTION_EXAMPLES.md # Concrete ❌/✅ examples for each principle
agent_docs/
  INDEX.md           # Table of contents for project-specific knowledge accumulated over time
```

## How it works

1. After install, open the project and let your agent read `CLAUDE.md`. It auto-imports the rules via `@./rules/...`.
2. On first run, the agent follows `rules/INIT.md` — asks you about stack, goals, and fills in the `[PLACEHOLDER]` fields in `CLAUDE.md`.
3. Once init is complete, **delete `rules/INIT.md` and remove its `@import` line from `CLAUDE.md`** so it stops being reused.
4. As work progresses, the agent persists project-specific knowledge (gotchas, infra quirks, standing rules) into `agent_docs/`.

## Updating

To pull a newer version of the rulebook:

```bash
npx degit --force snagreakim/rulebook/template .
```

⚠️ This **overwrites** skeleton files — including your customized `CLAUDE.md` and `agent_docs/INDEX.md`. Commit local changes first, then resolve the diff. Project-specific docs you've added to `agent_docs/` (e.g. `DB_SCHEMA_RULES.md`) are safe — degit only writes paths that exist in the template.

## License

MIT
