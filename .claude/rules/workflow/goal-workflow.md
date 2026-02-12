# Goal-Based Workflow

> **CRITICAL**: Follow these instructions at ALL times. Re-read after context compaction.

---

## Session Start Checklist

**ALWAYS do these at session start or after context compaction:**

1. **Read @.planning/GOALS.md** to identify current active goal
2. **If active goal exists**, read planning files from project root:
   - task_plan.md, findings.md, progress.md
3. **Read @.planning/BRANCHES.md** before any implementation work
4. **Check MILESTONES.md** if you need roadmap context
5. **Resume work** from where the last session left off

---

## Before Any Non-Trivial Work

**MANDATORY**: For any task that requires more than a few edits:

1. **A goal MUST exist** in `.planning/GOALS.md` before work begins
2. **Invoke `/planning-with-files`** to create planning files if they don't exist
3. The `planning-with-files` skill handles:
   - Creating `task_plan.md`, `findings.md`, `progress.md`
   - Auto-reading the plan before tool calls (hooks)
   - Reminding to update status after file changes
   - Verifying completion before session ends
   - The 3-Strike Error Protocol

This workflow adds the **goal registry and lifecycle** on top of that skill.

---

## Goal Tracking

### Two-Tier System

- **Milestones** (MILESTONES.md) = What we're building (features, roadmap)
- **Goals** (GOALS.md) = How we're building it (actionable work units)

### Planning Files Location

```
project root/                   (ACTIVE goal's files — created by planning-with-files)
├── task_plan.md
├── findings.md
├── progress.md

└── .planning/
    ├── GOALS.md               # Goal registry
    ├── MILESTONES.md          # Project roadmap
    ├── TECH_DECISIONS.md      # Technical decisions
    ├── BRANCHES.md            # Active branches across repos
    ├── history/goal-N/        # Completed goals
    └── iced/goal-N/           # Paused goals
```

---

## Starting a New Goal

1. **Update `.planning/GOALS.md`**:
   - Add new goal to registry table
   - Set it as "Active Goal"
   - Link to parent milestone

2. **Invoke the planning skill**:
   ```
   /planning-with-files "Goal #N: Description"
   ```
   This creates `task_plan.md`, `findings.md`, `progress.md` in project root.

---

## During Goal Work

The `planning-with-files` skill handles the discipline (auto-read plan, update status, log errors). Focus on the work.

---

## Icing a Goal (Temporary Pause)

Use when you need to pause work but plan to resume later.

1. **Update `.planning/GOALS.md`**:
   - Change status to "Iced"
   - Note reason for icing

2. **Move planning files**:
   ```bash
   mkdir -p .planning/iced/goal-N-description
   mv task_plan.md findings.md progress.md .planning/iced/goal-N-description/
   ```

3. **Clear "Active Goal"** in GOALS.md

---

## Resuming an Iced Goal

1. **Restore planning files**:
   ```bash
   mv .planning/iced/goal-N-description/*.md ./
   rmdir .planning/iced/goal-N-description
   ```

2. **Update GOALS.md**:
   - Set as "Active Goal"
   - Change status to "In Progress"

3. **Add session entry** to progress.md noting resumption

---

## Completing a Goal

1. **Mark phases complete** in task_plan.md

2. **Archive planning files**:
   ```bash
   mkdir -p .planning/history/goal-N-description
   mv task_plan.md findings.md progress.md .planning/history/goal-N-description/
   ```

3. **Update GOALS.md**:
   - Set status to "Complete"
   - Add completion date
   - Clear "Active Goal"

4. **Update MILESTONES.md** if milestone is complete

5. **Commit**:
   ```bash
   git add .planning/
   git commit -m "docs: complete Goal #N - Description"
   ```
