# Claude Code — Session Start

## First Steps (every session)

1. Read `@.planning/GOALS.md` to identify the **active goal**
2. If a goal is active, read its planning files: `task_plan.md`, `findings.md`, `progress.md`
3. Read `@.planning/BRANCHES.md` before any implementation work
4. Check `@.planning/MILESTONES.md` for roadmap context
5. Resume work from last checkpoint

## Where to Find Context

Do NOT guess or assume architecture — read the relevant document first.

| When you need... | Read this |
|------------------|-----------|
| Project architecture, repos, communication flow | `@.claude/rules/project-overview.md` |
| Project milestones & roadmap | `@.planning/MILESTONES.md` |
| Technical decisions and rationale | `@.planning/TECH_DECISIONS.md` |
| Active goal & goal registry | `@.planning/GOALS.md` |
| Active branches, base branches, worktrees | `@.planning/BRANCHES.md` |
| Implementation specs | `specs/` |
| Context documents | `context/` |
| Operational runbooks | `runbooks/` |

## Planning Workflow — `/planning-with-files` Skill

**IMPORTANT:** Always use the `/planning-with-files` skill to manage goal work. This is the core workflow.

- **Starting a goal:** After updating `GOALS.md`, invoke `/planning-with-files "Goal #N: Description"` to create `task_plan.md`, `findings.md`, `progress.md`
- **Resuming a goal:** If planning files already exist in project root, `/planning-with-files` will detect and resume them
- **Any non-trivial task** (>5 tool calls) should go through `/planning-with-files`
- Always read planning files before making changes
- Update `progress.md` after completing each phase
- Record discoveries in `findings.md`

## Context Integrity

- **Read before writing**: Always read a context/spec file before updating it
- **Update promptly**: When you discover new operational facts, update the relevant context document in the same session
- **Reference, don't duplicate**: If information belongs in a context doc, put it there and link to it
- **findings.md is for the active goal only**: Operational knowledge that outlasts the goal belongs in `context/` docs
- Specs are source of truth for component design
- Tech decisions are append-only (never edit existing entries retroactively)

## Working in Related Repos

- When making changes in related repos, log them in `progress.md` with repo, branch, and commit hash
- Update `@.planning/BRANCHES.md` when creating, merging, or abandoning branches
