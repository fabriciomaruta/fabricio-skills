---
name: pet-tester
description: >-
  Verifies workspace changes against the task and plan Verification section,
  writes artifacts/test-report.md with STATUS PASS or FAIL, and lists corrections
  for the planner. For HTTP APIs, starts the server and curls endpoints against
  the expected contract. For frontend UX, runs live Playwright via pet-playwright
  against ## Playwright Validation. Use when testing or verifying
  plan-execute-test output, or when the user asks to check whether the
  implementation passed.
---

# PET Tester

Personal/global skill. Contract lives under [pet-shared](../pet-shared/README.md).

## Instructions

1. Read the role: [../pet-shared/roles/tester.md](../pet-shared/roles/tester.md)
2. Follow the report template: [../pet-shared/schema/test-report.schema.md](../pet-shared/schema/test-report.schema.md)
3. Run verification commands from the plan and the original task
4. Write only the report artifact (usually `artifacts/test-report.md`)
5. Do not fix product code

## HTTP APIs (required)

When the work is an HTTP API:

1. Start the app environment (sync deps + run the server from the plan/task).
2. Hit endpoints with `curl` and assert status + body match the API contract.
3. Fail (`STATUS: FAIL`) on any unexpected response; do not rely only on unit tests.
4. Include curl commands and response evidence in the report; stop the server when done.

## Frontend / Playwright (required for UI)

When the work includes frontend UX (or the plan has a non-empty
`## Playwright Validation`):

1. Follow [../pet-playwright/SKILL.md](../pet-playwright/SKILL.md).
2. Do not skip live Playwright for UI work (`npm run test:parse` alone is not enough).
3. Include the checklist path and per-PV evidence under `## Playwright` in the report.

## Status line

The report must include exactly one of:

```
STATUS: PASS
```

```
STATUS: FAIL
```
