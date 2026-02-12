# Documentation Guidelines

## When to Update Which File

| Change Type | Update |
|-------------|--------|
| Architecture change | `@.planning/TECH_DECISIONS.md` |
| New milestone | `@.planning/MILESTONES.md` (add status) |
| New goal | `@.planning/GOALS.md` (add to registry) |
| Goal complete | Archive to `.planning/history/`, update `@.planning/GOALS.md` |
| Milestone complete | Update `@.planning/MILESTONES.md` status |
| Branch created/merged/abandoned | `@.planning/BRANCHES.md` |
| Operational discovery | Relevant `context/` document |
| Design decision | `@.planning/TECH_DECISIONS.md` as new TD-XXX |
| Implementation details | Relevant `specs/` document |

## Technical Decisions

Record major decisions in @.planning/TECH_DECISIONS.md:
- Architecture changes
- Technology choices
- Security decisions
- Design pattern adoption

Use format: `TD-XXX: Decision Name (YYYY-MM-DD)`

## Principles

- **Reference over duplication**: Link to existing docs, don't copy
- **Keep docs current**: Update when making changes
- **Goal deliverables**: Each must be actionable and complete
- **Milestone deliverables**: Each must be user-visible and complete
