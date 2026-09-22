# Plan → Execute → Test

Three agent roles form a closed loop. The planner may **split** a task into subtasks; independent subtasks run **in parallel** (each with its own loop of max 3); a **final integration test** validates the whole task.

## Roles

| Role | Definition | Writes | Must not |
|------|------------|--------|----------|
| Planner | [roles/planner.md](roles/planner.md) | `artifacts/plan.md` | Edit product code |
| Executor | [roles/executor.md](roles/executor.md) | Workspace code/files | Rewrite the parent plan or write the test report |
| Tester | [roles/tester.md](roles/tester.md) | `test-report.md` (subtask or final) | Fix product code |

## Artifact schemas

- Plan: [schema/plan.schema.md](schema/plan.schema.md) — `Strategy`, `Subtasks`, `Integration Verification`, `Playwright Validation`
- Test report: [schema/test-report.schema.md](schema/test-report.schema.md)

Reports must include a line `STATUS: PASS` or `STATUS: FAIL`.

UI verification uses Playwright against the plan's Playwright Validation checklist (`playwright-pet` / pet-playwright).

## Surfaces

Install via the [skills CLI](https://github.com/vercel-labs/skills) into Cursor,
Copilot, OpenCode, Claude Code, Codex, and other compatible agents (see the
repo root README). Skills live as sibling folders so links to `pet-shared`
resolve after install.

## Loop

```
task → planner → plan.md (single | split)
         ↓
    ┌────┴────┐  parallel when DependsOn allows
    ST-a loop  ST-b loop   (each: plan→execute→test, max 3)
    └────┬────┘
         ↓ all PASS
   final integration tester → test-report.md
         ↓ FAIL (outer max 3)
      planner (retry)
```

Default max iterations: **3** for the outer loop and for **each** subtask loop.
