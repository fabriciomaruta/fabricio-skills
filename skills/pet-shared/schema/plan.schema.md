# Plan

## Goal
<!-- One sentence: what success looks like for the overall task -->

## Constraints
<!-- Hard limits: files in/out of scope, languages, no-touch areas -->

## Strategy
<!-- Exactly one of: single | split -->
single

## Subtasks

<!-- Always define at least one subtask. For Strategy single, use one ST block.
     For Strategy split, prefer independent file ownership; use DependsOn when order matters. -->

### ST-1: main
- **Goal:** <!-- subtask success -->
- **Constraints:** <!-- optional; inherits overall Constraints if omitted -->
- **DependsOn:** <!-- comma-separated ST ids, or empty -->
- **Steps:**
  1. <!-- actionable step -->
- **Files:**
  | Path | Action | Notes |
  |------|--------|-------|
  | <!-- path --> | create \| edit \| delete | <!-- why --> |
- **Verification:** <!-- how to confirm THIS subtask alone -->

<!-- Additional blocks when Strategy is split:
### ST-2: other
- **Goal:** ...
- **DependsOn:** ST-1
- **Steps:** ...
- **Files:** ...
- **Verification:** ...
-->

## Integration Verification
<!-- Whole-task checks AFTER every subtask has STATUS: PASS.
     Include commands, expected HTTP/curl contracts when applicable. -->

## Playwright Validation
<!-- Required when the task touches frontend UX. Empty or "N/A" only when there is no UI.
     Table of flows the tester must run with Playwright (stable IDs). -->
| ID | Flow | Steps | Expected |
|----|------|-------|----------|
| <!-- PV-... --> | <!-- short name --> | <!-- user actions --> | <!-- observable outcome --> |

## Notes
<!-- Optional: risks, assumptions, why split vs single -->
