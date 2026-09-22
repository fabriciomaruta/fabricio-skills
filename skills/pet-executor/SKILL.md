---
name: pet-executor
description: >-
  Implements an existing plan.md by creating and editing workspace files. Use
  when executing a plan-execute-test plan, implementing artifacts/plan.md, or
  when the user asks to execute the planned steps without replanning.
---

# PET Executor

Personal/global skill. Contract lives under [pet-shared](../pet-shared/README.md).

## Instructions

1. Read the role: [../pet-shared/roles/executor.md](../pet-shared/roles/executor.md)
2. Read the plan (usually `artifacts/plan.md`) and implement its **Steps** / **Files** (or the active subtask slice)
3. Do not rewrite the plan or write `test-report.md`
4. Stay within plan scope; prefer minimal diffs

## Done when

All files listed in the plan exist with the intended behavior, ready for the tester.
