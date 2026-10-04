# First-Time Initialization

This file applies **only when the project is first being set up**. Once init is complete, delete this file and remove its reference from `AGENTS.md` and its `@import` line from `CLAUDE.md` so the boilerplate stops being reused in context.

## Process
- Ask the user what to build from scratch. The user is expected to use PLAN MODE; if not, guide them to it.
- First-time init means there is only a skeleton `./AGENTS.md`. While in PLAN MODE, ask the user about important parts to make explicit specs/guidelines and enrich `AGENTS.md`. If the user installed `grill-with-docs`, offer to run it for this interview. It records terms in `GLOSSARY.md` and hard decisions in `docs/adr/` as it goes.
- When initializing the project, do not create files by yourself. Use commands instead.

## Examples

Scaffolders that refuse a non-empty folder (such as `create-next-app`) fail here, because the template files are already in the folder. Scaffold into a subfolder, then merge it up.

next-js app:
```bash
pnpm create next-app@latest app-tmp --yes --disable-git --skip-install
cat app-tmp/AGENTS.md >> AGENTS.md          # keep the template's AGENTS.md, append Next.js's agent block
rm app-tmp/AGENTS.md app-tmp/CLAUDE.md      # the template's CLAUDE.md already imports AGENTS.md
cp -R app-tmp/. . && rm -r app-tmp
pnpm install
```
- `--disable-git` stops a nested `.git/` from overwriting the project's repository.
- Before `cp`, check `app-tmp/` for other files that would overwrite template files. Ask the user if any collide.

python app:
```bash
uv init
uv add --dev ty
uv sync
```

## Skills

Skills are not bundled with this template. Fetch them at init so the project gets the latest version. Show the user this list and ask which ones to install. Install only what the user picks.

| Skill | What it does | Source |
|---|---|---|
| `asd-ste100` | Rewrites English text into Simplified Technical English on request. Ships the linter that `OUTPUT_STYLE.md` uses. | https://github.com/danyuchn/asd-ste100-skill |
| `grill-me` | `/grill-me`: interviews the user in rounds to sharpen a plan. Needs `grilling`. | https://github.com/mattpocock/skills/tree/main/skills/productivity/grill-me |
| `grill-with-docs` | `/grill-with-docs`: the same interview, and it writes `GLOSSARY.md` and ADRs as terms and decisions settle. Needs `grilling` and `domain-modeling`. | https://github.com/mattpocock/skills/blob/main/docs/engineering/grill-with-docs.md |
| `drawio-skill` | `/drawio-skill`: creates and edits `.drawio` diagrams. About 1 MB with ~50 Python scripts. Native export needs the draw.io app. | https://github.com/Agents365-ai/drawio-skill |

Install with the `skills` CLI. It writes the skill to `.agents/skills/` (read by Codex) and symlinks it into `.claude/skills/` (read by Claude Code):

```bash
npx skills add danyuchn/asd-ste100-skill -a claude-code codex -y
npx skills add mattpocock/skills --skill grilling grill-me -a claude-code codex -y
npx skills add mattpocock/skills --skill grilling domain-modeling grill-with-docs -a claude-code codex -y
npx skills add Agents365-ai/drawio-skill -a claude-code codex -y
```

- Always install the dependencies listed in the table. `grill-me` and `grill-with-docs` are one-line wrappers and do nothing alone.
- Ask whether to commit `.agents/`, `.claude/skills/`, and `skills-lock.json`. Committing them pins the versions for teammates. `npx skills update` pulls newer versions later.
- If the user declines `asd-ste100`, skip the linter step in `OUTPUT_STYLE.md`.

## Tools

CLI tools that are not skills. Ask the user whether to install each one.

| Tool | What it does | Source |
|---|---|---|
| Backlog.md | Local issue tracker. Each task is a Markdown file in `backlog/`, with status and dependencies on other tasks. Use it for open questions, deferred discussions, and work that waits on other work. | https://github.com/MrLesk/Backlog.md |

Install and initialize Backlog.md:

```bash
npm i -g backlog.md        # or: brew install backlog-md
backlog init "<project name>" --defaults --agent-instructions agents --integration-mode cli
```

- `--agent-instructions agents` appends a guideline block to `AGENTS.md` and keeps the existing content. Do not also pick `claude`. `CLAUDE.md` already imports `AGENTS.md`, so a second block would duplicate it.
- If the project has no git remote yet, run `backlog config set remoteOperations false` to silence the remote warning.

## Checklist
- [ ] Initial project scaffold + minimum spec/plan/goal is fully equipped
- [ ] Skeleton `AGENTS.md` updated
- [ ] `[PROJECT NAME]` and the description in `GLOSSARY.md` filled in
- [ ] Skills and tools chosen by the user installed
- [ ] Delete `./rules/INIT.md`, remove its reference from `AGENTS.md`, and remove its `@import` from `CLAUDE.md`
