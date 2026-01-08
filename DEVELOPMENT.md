# Development Workflow Guide

This document defines the working agreement between developer and Cline AI. It ensures clear tasks, stable results, and prevents typical AI-agent errors.

## Daily Routine (Cline + Python, Cross-Platform)

1. Open repository, check `git status` (clean preferred)
2. Switch to or create feature branch
3. **Baseline check**: Run `.\scripts\check.ps1` (must be green before Cline makes changes)
4. Start Cline → **Plan Mode**: Define goal + acceptance criteria + non-goals → Create plan (max 10 steps)
5. Review plan (scope, tests, risks). Only then switch to **Act Mode**
6. **Act Mode**: Implement only 1 plan step at a time (no side refactors)
7. After each step: Run `.\scripts\check.ps1` (if red → minimal fix only, no extras)
8. When green: Small, focused commit
9. If context/scope drifts: Return to **Plan Mode**, refine plan or create new task + update memory-bank
10. **PR**: Cline writes PR description, you review + final `.\scripts\check.ps1` check

## Definition of Done (DoD)

A task is "done" when:

- **Defined behavior is implemented** (acceptance criteria met)
- **Quality gate is green**:
  - `.\scripts\check.ps1` passes
- **New/changed logic has tests**:
  - Feature: at least 1 test per new behavior
  - Bugfix: Regression test that would fail before the fix
- **No side refactors** (only changes that belong to the task)
- **No new dependencies** without explicit approval
- **Changes are committed** in small, comprehensible units

## Quality Gate (Single Source of Truth)

Always run:
```powershell
.\scripts\check.ps1
```

This script checks:
- **Ruff (lint)**: `ruff check .`
- **Ruff (format check)**: `ruff format --check .`
- **Tests**: `pytest`

**Why `.\scripts\check.ps1`**: A single command that runs robustly on Windows and Linux (uses explicit `.venv\Scripts\python.exe` and `.venv\Scripts\ruff.exe` paths). It provides a clear exit code and is ideal for CI, VS Code tasks, and Cline.

## Cline Scope & Safety

- **Standard**: Plan Mode first. Act Mode only after plan approval
- **Cline may only work within the workspace** (no files outside the project folder)
- **No new dependencies** without explicit approval from me
- **No changes to project/tooling configuration** (`pyproject.toml`, CI, pre-commit, etc.) without asking first
- **No side refactors**. Only changes that belong to the task
- **Terminal**: Only execute commands after I approve them
- **Before each tool run**: Ensure the `.venv` is being used (sys.executable check if unclear)

## Working Method (Plan/Act)

### Plan Mode:
- Create a numbered plan (max 10 steps)
- Each step specifies: affected files + test/check afterwards

### Act Mode:
- Implement only 1 plan step per iteration (max 2 if I explicitly say so)
- After each step: Brief summary "What changed?" + exactly 1 command for quality gate
- If scope grows: Return to Plan Mode and update the plan

## Standard Non-Goals (unless explicitly requested)

- No renamings beyond the feature scope
- No formatting/lint-fixing in unrelated files
- No architectural refactors "on the side"
- No new libraries
- No changes to public APIs without consultation

## Prompt Templates

These three cover 80% of the work.

### Plan Kickoff (Plan Mode):
```
Plan Mode.
Goal: …
Acceptance criteria: …
Non-goals: …
Please read the relevant files and create a numbered plan (max 10 steps).
Each step: affected files + test/check afterwards.
```

### Implement Step 1 (Act Mode):
```
Act Mode. Implement only Step 1 from the plan.
No side refactors, no new dependencies.
Afterwards: Briefly say what changed and name exactly 1 command for the quality gate.
```

### Debug Minimal (Act Mode):
```
Act Mode. Analyze only the error output.
Suggest the smallest change that fixes the error.
Then run quality gate again (only after my approval).
```

## Real Repository Workflow

### Branch & Baseline

1. **Branch per task**: `git checkout -b feature/<name>` or `fix/<name>`
2. **Baseline check**: Always run `.\scripts\check.ps1` before Cline work
3. **If baseline is red**: Fix baseline first (don't debug "wrong causes")

### Plan Mode Template

```
Plan Mode.
Goal: …
Acceptance criteria: …
Non-goals: …
Read only the relevant files (I'll link them) and create a plan (max 10 steps).
Each step: affected files + then .\scripts\check.ps1.
No changes yet.
```

### Act Mode Template

```
Act Mode. Implement only Step 1 from the plan.
No side refactors, no new dependencies.
Afterwards: run .\scripts\check.ps1 (only after my approval).
```

### Commit Discipline

- **One logical change per commit**: "Core function + tests", "CLI flag + tests"
- **If Cline produces large diff**: Roll back, redo in smaller steps
- **No "whole repo reformatting"** in feature commits
- **Commit message schema**: 
  - `feat(core): add new feature`
  - `fix(cli): resolve parameter issue`
  - `test: add regression test for bug`
  - `chore: update configuration`

### PR Preparation

- **When green**: Create PR with motivation, changes, tests, risks
- **Review checklist**:
  1. Behavior matches acceptance criteria
  2. Tests are meaningful (would fail if broken)
  3. Scope maintained (no "side work")
  4. `.\scripts\check.ps1` is green
  5. Changes well-sliced (not one giant AI commit)

### Merge Conflict Resolution

- **Scope restriction**: "Act Mode. Resolve only the merge conflict markers in these files."
- **No refactors/renaming** during conflict resolution
- **After fix**: `git add -A`, `git commit -m "chore: resolve merge conflicts"`
- **If check.ps1 red after**: Minimal fix only, no architectural cleanup

### Refactor Sprints (controlled)

- **Constraints**: Public API unchanged, behavior identical, tests green after each step
- **Phases**:
  1. Mechanical restructuring
  2. Unification/naming (only if needed)
  3. Performance/fine-tuning (optional)
- **Prompt**: "Plan Mode. Refactor goal: … Constraints: no API changes, behavior must remain identical. Plan in phases, each step must be green after .\scripts\check.ps1."

## Cross-Platform Notes

### Windows
- Use PowerShell (built-in)
- Virtual environment: `.venv\Scripts\`
- Python: `.\.venv\Scripts\python.exe`
- Ruff: `.\.venv\Scripts\ruff.exe`

### Linux/macOS
- Use `pwsh` (PowerShell Core) or Bash fallback (`scripts/check.sh`)
- Virtual environment: `.venv/bin/`
- Python: `./.venv/bin/python`
- Ruff: `./.venv/bin/ruff`

## Output Format (for transparency)

After each Act Mode step:

1. List exactly which files changed
2. Summarize what changed in 3-6 bullet points
3. Propose exactly ONE command to run next (usually `.\scripts\check.ps1`)
4. If errors occur: Quote only the relevant error section and propose the minimal fix

## Quality Gate Failure Response

When `.\scripts\check.ps1` fails:

1. **Analyze** only the error output
2. **Identify** the minimal change needed
3. **Propose** the fix
4. **Wait** for approval
5. **Implement** only that fix
6. **Run** quality gate again

**Do NOT**:
- Fix other issues "while you're at it"
- Refactor unrelated code
- Change formatting in other files
- Add features not related to the error

---

*This working agreement is effective immediately for all Cline interactions in this project.*
*Last updated: 2026-01-08*
