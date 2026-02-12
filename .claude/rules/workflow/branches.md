# Branch Management

## Branch Registry

**Read `.planning/BRANCHES.md` before any implementation phase.** It tracks active branches, base branches, and worktree paths across all repos.

## Rules

- **Check before coding**: Know which repo + branch + base you're working in
- **Plan branches per phase**: Each implementation phase in `task_plan.md` should note which branches it touches
- **Update on change**: Create, merge, or abandon a branch → update BRANCHES.md in the same session
- **Log in progress.md**: Note branch operations (checkout, push, merge) with repo + commit hash
- **Worktree column**: Populate when using git worktrees; leave empty when using standard checkout
