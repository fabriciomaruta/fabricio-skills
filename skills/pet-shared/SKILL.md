---
name: pet-shared
description: >-
  Shared Plan-Execute-Test (PET) contract: role definitions and artifact
  schemas used by pet-planner, pet-executor, pet-tester, and pet-loop. Install
  alongside the other PET skills so relative role/schema links resolve.
---

# PET Shared

Canonical contract for the plan → execute → test loop. Other PET skills link
here; install this skill together with them.

## Contents

| Path | Purpose |
|------|---------|
| [README.md](README.md) | Overview, surfaces, loop diagram |
| [roles/planner.md](roles/planner.md) | Planner role |
| [roles/executor.md](roles/executor.md) | Executor role |
| [roles/tester.md](roles/tester.md) | Tester role |
| [schema/plan.schema.md](schema/plan.schema.md) | `artifacts/plan.md` template |
| [schema/test-report.schema.md](schema/test-report.schema.md) | `artifacts/test-report.md` template |

## Companion skills

- `pet-planner` — writes the plan
- `pet-executor` — implements the plan
- `pet-tester` — verifies and reports
- `pet-playwright` — live UI acceptance for frontend work
- `pet-loop` — full outer/subtask orchestration
