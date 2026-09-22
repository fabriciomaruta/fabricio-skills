---
name: pet-planner
description: >-
  Analyzes a coding task and writes a concrete implementation plan to
  artifacts/plan.md without editing product code. May set Strategy single or
  split into Subtasks for divide-and-conquer. Use when planning work in the
  plan-execute-test loop, when the user asks to plan before implementing, or when
  revising a plan from a failed test-report.
---

# PET Planner

Personal/global skill. Contract lives under [pet-shared](../pet-shared/README.md).

## Instructions

1. Read the role: [../pet-shared/roles/planner.md](../pet-shared/roles/planner.md)
2. Follow the plan template: [../pet-shared/schema/plan.schema.md](../pet-shared/schema/plan.schema.md)
3. Inputs: the user task (and prior `artifacts/test-report.md` / subtask reports if this is a retry)
4. Write only `artifacts/plan.md` (or the path the user specifies)
5. Do not edit product source code

## Strategy

- `single` — one `### ST-...` subtask
- `split` — multiple subtasks; prefer parallel-safe file ownership; use `DependsOn` when needed
- Always fill **Integration Verification** for the whole task

## Frontend / Playwright

- When the task touches frontend UX, the plan **MUST** include
  `## Playwright Validation` with concrete PV rows (see plan schema:
  ID | Flow | Steps | Expected, e.g. `PV-load`, `PV-create`).
- **Integration Verification** must reference running that checklist via
  Playwright / [pet-playwright](../pet-playwright/SKILL.md).
- If there is no UI: set Playwright Validation to **N/A** with a one-line
  reason.

## Retry

If a prior report has `STATUS: FAIL`, incorporate every item under **Corrections for Planner** into the revised plan.
