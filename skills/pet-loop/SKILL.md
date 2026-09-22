---
name: pet-loop
description: >-
  Runs the full plan-execute-test loop in chat: plan (optional split into
  subtasks), parallel per-subtask execute/test loops (max 3 each), then a final
  integration test. Use only when the user explicitly asks for the PET loop,
  plan-execute-test loop, or automated plan-implement-verify cycle.
disable-model-invocation: true
---

# PET Loop

Personal/global skill. Orchestrate planner → (parallel subtask execute/test loops) → final integration test.

## Setup

1. Resolve the task (user message or a `task.md` path)
2. Choose an artifacts directory (default: `artifacts/` under the app or cwd)
3. Set `max_iterations` (default: **3**) for the **outer** loop and for **each subtask** loop unless the user overrides
4. Read the contract: [../pet-shared/README.md](../pet-shared/README.md)

## Outer loop

For `outer` from 1 to `max_iterations`:

### 1. Plan

Follow [../pet-planner/SKILL.md](../pet-planner/SKILL.md).

- Inputs: task + prior `test-report.md` if `outer > 1`
- Output: `artifacts/plan.md` with `Strategy`, `Subtasks`, and `Integration Verification`

### 2. Subtask loops (divide and conquer)

Parse every `### ST-<id>:` block from the plan.

- Build a dependency graph from each subtask's **DependsOn**
- While subtasks remain:
  - Select all subtasks whose deps have `STATUS: PASS` (or no deps) and are not yet started
  - Run those subtasks **in parallel** (separate conversation branches / Task subagents / tool batches)
  - Each parallel unit runs a **sub-loop of size `max_iterations` (default 3)**:

#### Sub-loop for `ST-<id>` (max 3)

Artifacts under `artifacts/subtasks/ST-<id>/`:

1. **Plan (sub)** — iteration 1: write `plan.md` as the slice for this ST from the parent plan. Later iterations: follow pet-planner to revise **only this subtask** using its prior `test-report.md`.
2. **Execute** — follow [../pet-executor/SKILL.md](../pet-executor/SKILL.md) for that subtask plan only.
3. **Test** — follow [../pet-tester/SKILL.md](../pet-tester/SKILL.md); write `artifacts/subtasks/ST-<id>/test-report.md`.
4. If `STATUS: PASS` → subtask done. If `FAIL` and iterations remain → next sub-iteration. If exhausted → **outer FAIL** (do not start dependents).

### 3. Final integration test

Only when **every** subtask has `STATUS: PASS`:

- Follow pet-tester against the **original task** + plan **Integration Verification**
- Write `artifacts/test-report.md`
- Include HTTP/curl checks when the task is an HTTP API
- When the task/plan has frontend UX or a non-empty `## Playwright Validation`,
  the tester **must** follow [pet-playwright](../pet-playwright/SKILL.md)
  (live Playwright), not only curl/unit tests or `npm run test:parse`

### 4. Decide (outer)

- If final report is `STATUS: PASS` → stop and summarize success
- If `STATUS: FAIL` and outer iterations remain → next outer iteration (planner reads the final report)
- If `STATUS: FAIL` and no outer iterations left → stop and summarize remaining failures

## Rules

- Keep role boundaries: planner does not code; tester does not fix code
- Persist parent plan, each subtask's plan/report, and the final test-report on disk
- Tell the user which outer iteration / subtask / sub-iteration is running and the final STATUS
- Prefer parallel execution for independent subtasks; honor DependsOn

## Multi-task (outside this skill)

For **several independent tasks** in parallel, do not run multiple `/pet-loop`
chats in the same workspace without isolation. Prefer separate worktrees or
one Task subagent per task (each with its own `artifacts/` dir), then aggregate
STATUS lines into a summary.
