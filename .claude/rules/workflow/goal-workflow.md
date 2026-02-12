# Goal-Based Workflow

**Core tool:** The `/planning-with-files` skill creates and manages the planning files (`task_plan.md`, `findings.md`, `progress.md`). Always use it when starting or resuming goals.

## Session Start Checklist

1. Read `@.planning/GOALS.md` to identify active goal
2. If goal exists → invoke `/planning-with-files` to resume (it detects existing files)
3. If no planning files exist yet → invoke `/planning-with-files "Goal #N: Description"` to create them
4. Check `MILESTONES.md` for roadmap context
5. Resume work from last session

## Two-Tier Goal System

- **Milestones** (MILESTONES.md) = What we're building (features, roadmap)
- **Goals** (GOALS.md) = How we're building it (actionable work units)

## Planning Files Location

```
project root/                   (ACTIVE goal's files)
├── task_plan.md
├── findings.md
├── progress.md

└── .planning/
    ├── GOALS.md               # Goal registry
    ├── MILESTONES.md          # Project roadmap
    ├── TECH_DECISIONS.md      # Technical decisions
    ├── history/goal-N/        # Completed goals
    └── iced/goal-N/           # Paused goals
```

## Starting a New Goal

1. Update `.planning/GOALS.md` — add to registry, set as "Active Goal"
2. Invoke `/planning-with-files "Goal #N: Description"` — **this is mandatory**
3. The skill creates `task_plan.md`, `findings.md`, `progress.md` in project root
4. Work through phases defined in `task_plan.md`, updating `progress.md` and `findings.md` as you go

## Icing a Goal (pause)

```bash
mkdir -p .planning/iced/goal-N-description
mv task_plan.md findings.md progress.md .planning/iced/goal-N-description/
# Update GOALS.md status to "Iced"
```

## Completing a Goal

```bash
mkdir -p .planning/history/goal-N-description
mv task_plan.md findings.md progress.md .planning/history/goal-N-description/
# Update GOALS.md status to "Complete"
git add .planning/
git commit -m "docs: complete Goal #N - Description"
```

## Resuming an Iced Goal

```bash
mv .planning/iced/goal-N-description/{task_plan,findings,progress}.md .
# Update GOALS.md status to "In Progress"
```
