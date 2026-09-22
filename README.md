# fabricio-skills

Agent skills for the **Plan → Execute → Test (PET)** loop. Install once and use
from Cursor, GitHub Copilot, OpenCode, Claude Code, Codex, and other agents that
speak the [Agent Skills](https://skills.sh/) format.

## Skills in this repo

| Skill | Role |
|-------|------|
| `pet-loop` | Full outer/subtask PET orchestration (invoke explicitly) |
| `pet-planner` | Writes `artifacts/plan.md` (no product code) |
| `pet-executor` | Implements an existing plan |
| `pet-tester` | Verifies work; writes `artifacts/test-report.md` |
| `pet-playwright` | Live Playwright UI acceptance for frontend tasks |
| `pet-shared` | Shared roles + plan/report schemas (install with the others) |

Install **all** of them together so relative links under `pet-shared/` resolve.

## Quick install (recommended)

After this repo is on GitHub (replace `fabriciomaruta`):

```bash
# Interactive — pick agents + skills
npx skills add fabriciomaruta/fabricio-skills

# Non-interactive: all PET skills, global, common agents
npx skills add fabriciomaruta/fabricio-skills \
  --skill '*' \
  -g \
  -a cursor -a github-copilot -a opencode -a claude-code -a codex \
  -y
```

Before the GitHub remote exists, install from a local clone:

```bash
git clone <this-repo> ~/dev/fabricio-skills   # or use the path you already have
npx skills add ~/dev/fabricio-skills --skill '*' -g -y
```

List what the CLI discovers:

```bash
npx skills add fabriciomaruta/fabricio-skills --list
# or
npx skills add ~/dev/fabricio-skills -l
```

## Install by agent

The [skills CLI](https://github.com/vercel-labs/skills) detects agents and writes
into their skill directories (symlink by default; use `--copy` if needed).

| Agent | CLI flag | Typical global path |
|-------|----------|---------------------|
| Cursor | `-a cursor` | `~/.cursor/skills/` |
| GitHub Copilot | `-a github-copilot` | `~/.copilot/skills/` (project: `.agents/skills/` or `.github/skills/`) |
| OpenCode | `-a opencode` | `~/.config/opencode/skills/` |
| Claude Code | `-a claude-code` | `~/.claude/skills/` |
| Codex | `-a codex` | `~/.codex/skills/` |

### Cursor

```bash
npx skills add fabriciomaruta/fabricio-skills --skill '*' -g -a cursor -y
```

Then invoke in chat, e.g. `/pet-loop`, or ask the agent to use `pet-planner`.

Project-scoped (committed with the app):

```bash
cd your-app
npx skills add fabriciomaruta/fabricio-skills --skill '*' -a cursor -y
```

### GitHub Copilot

```bash
npx skills add fabriciomaruta/fabricio-skills --skill '*' -g -a github-copilot -y
```

In Copilot CLI you can manage skills with `/skills`. Project skills may also live
under `.github/skills/` or `.agents/skills/` — the CLI places them correctly when
you target `github-copilot`.

### OpenCode

```bash
npx skills add fabriciomaruta/fabricio-skills --skill '*' -g -a opencode -y
```

### Claude Code / Codex

```bash
npx skills add fabriciomaruta/fabricio-skills --skill '*' -g -a claude-code -y
npx skills add fabriciomaruta/fabricio-skills --skill '*' -g -a codex -y
```

### All detected agents at once

```bash
npx skills add fabriciomaruta/fabricio-skills --skill '*' --all -y
```

## Manual install (no CLI)

Clone or copy the skill folders so each agent sees siblings (needed for
`../pet-shared` links):

```text
<agent-skills-root>/
  pet-loop/
  pet-planner/
  pet-executor/
  pet-tester/
  pet-playwright/
  pet-shared/
```

Examples:

```bash
# Cursor (global)
cp -R skills/pet-* ~/.cursor/skills/

# Copilot (global)
mkdir -p ~/.copilot/skills && cp -R skills/pet-* ~/.copilot/skills/

# OpenCode (global)
mkdir -p ~/.config/opencode/skills && cp -R skills/pet-* ~/.config/opencode/skills/

# Claude Code (global)
mkdir -p ~/.claude/skills && cp -R skills/pet-* ~/.claude/skills/
```

## How PET works

```
task → planner → plan.md (single | split)
         ↓
    per-subtask execute → test (max 3 each; parallel when DependsOn allows)
         ↓
   final integration test → test-report.md (STATUS: PASS | FAIL)
```

Details: [skills/pet-shared/README.md](skills/pet-shared/README.md).

## Publish this repo to GitHub

This directory is already a local git repo with an initial commit. Create the
remote when ready:

```bash
cd ~/dev/fabricio-skills

# If gh is installed and authenticated:
gh repo create fabricio-skills --public --source=. --remote=origin --push

# Or manually:
# 1. Create an empty repo on GitHub named fabricio-skills
# 2. Then:
git remote add origin git@github.com:fabriciomaruta/fabricio-skills.git
git push -u origin main
```

After push, anyone can install with:

```bash
npx skills add fabriciomaruta/fabricio-skills --skill '*' -g -y
```

## Updates

```bash
npx skills update
# or re-add from the remote
npx skills add fabriciomaruta/fabricio-skills --skill '*' -g -y
```

## License

Use and adapt freely for your own agent workflows.
