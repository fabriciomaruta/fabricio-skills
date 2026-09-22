---
name: pet-playwright
description: >-
  Runs PET frontend validation with Playwright against a Playwright Validation
  checklist from plan.md. Use when testing UI work in the plan-execute-test
  loop, when a plan has ## Playwright Validation, or when the user asks to
  validate frontend flows with Playwright.
---

# PET Playwright

Personal/global skill for live UI acceptance in the plan-execute-test loop.
Harness docs live in the workspace: `playwright-pet/README.md`.

## Checklist format

Plans and `artifacts/playwright-validation.md` use a markdown table under
`## Playwright Validation`:

| ID | Flow | Steps | Expected |
|----|------|-------|----------|
| PV-load | Load app | Open `/` | Notes UI renders |
| PV-create | Create note | Fill form; submit | New note appears |

Use stable ids like `PV-load`, `PV-create`, etc.

## Instructions

1. **Read the checklist** from the active plan (or task): section
   `## Playwright Validation`. If the work is UI-related and this section is
   missing → treat as FAIL guidance for the planner (report `STATUS: FAIL`
   and ask the planner to add concrete PV rows).

2. **Write/refine** the checklist to `artifacts/playwright-validation.md`
   (copy the table from the plan). The tester may clarify Steps, but must keep
   the stable PV ids from the plan.

3. **Ensure UI + API are up** before live runs. For pydantic-ai-notes, prefer
   documenting and using start commands such as:
   - Backend: `uv run uvicorn app.main:app --host 127.0.0.1 --port 8000`
     (from `pydantic-ai-notes/backend/`)
   - Frontend: Vite on `:5173` (from `pydantic-ai-notes/frontend/`)
   - Or: docker compose for the stack
   Record the chosen start commands in the test report.

4. **Run Playwright** from repo root `playwright-pet/`:
   - `npm install` if needed
   - `npx playwright install chromium` if needed
   - Live acceptance (adjust paths relative to where the checklist was written):

     ```bash
     BASE_URL=http://127.0.0.1:5173 \
       CHECKLIST_PATH=../artifacts/playwright-validation.md \
       npm test
     ```

5. **Record evidence** in the test report under `## Playwright`: per-PV
   PASS/FAIL plus relevant command output.

6. **Any failed PV** → overall `STATUS: FAIL`.

7. **Offline is not enough:** `npm run test:parse` alone does **not** satisfy
   final UI acceptance. Live Playwright against a running UI is required for
   frontend tasks.

8. Point to harness README: `playwright-pet/README.md`.
