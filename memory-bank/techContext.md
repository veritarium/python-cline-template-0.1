# Tech Context

## Environment
- **OS**: Windows / Linux / macOS (cross-platform)
- **Shell**: PowerShell (Windows), Bash (Linux/macOS)
- **Python**: 3.12+
- **Virtual Environment**: `.venv/` in repo root

## Quality Gate
Single command to verify everything:
```powershell
.\scripts\check.ps1
```

This runs:
- `ruff check .` - Linting
- `ruff format --check .` - Format verification
- `pytest` - All tests

## Key Commands

### Setup
```powershell
# Create virtual environment
py -m venv .venv                           # Windows
python3 -m venv .venv                      # Linux/macOS

# Install dependencies
.\.venv\Scripts\python.exe -m pip install -U pip ruff pytest    # Windows
./.venv/bin/python -m pip install -U pip ruff pytest            # Linux/macOS
```

### Development
```powershell
# Quality gate (always before committing)
.\scripts\check.ps1

# Auto-fix issues
.\scripts\fix.ps1

# Run tests only
.\.venv\Scripts\python.exe -m pytest tests/     # Windows
./.venv/bin/python -m pytest tests/             # Linux/macOS
```

## Testing
- **Framework**: pytest
- **Test location**: `tests/`
- **Test naming**: `test_*.py`
- **Run with**: `pytest` or `python -m pytest`

## Linting/Formatting
- **Tool**: Ruff (configured in `pyproject.toml`)
- **Line length**: 100
- **Rules**: E, F, I, B, UP (see pyproject.toml for full config)

## Troubleshooting Ladder

### Level 1: Gather Facts (2 minutes)
In the repo root, run:
```powershell
git status
.\.venv\Scripts\python.exe -c "import sys; print(sys.executable)"
.\.venv\Scripts\python.exe -m pip --version
.\scripts\check.ps1
```

### Level 2: Wrong venv / Wrong Python (Most Common Issue)
**Symptoms:**
- pytest can't find packages
- ruff not found
- Behavior is "different than yesterday"

**Fix:**
- Always use venv executables explicitly:
  - `.\.venv\Scripts\python.exe ...`
  - `.\.venv\Scripts\ruff.exe ...`
- In VS Code: Re-select interpreter (Python: Select Interpreter) → choose `.venv`

### Level 3: Cline Can't Read Terminal Output / Hangs
**Symptoms:**
- Cline waits forever for output
- Commands run but Cline "doesn't see" the result

**Fix Steps:**
1. Restart VS Code
2. In Cline Settings: Set Terminal Execution Mode to "Background Exec"
3. Avoid inline commands with complex quoting
4. Use scripts instead: `.\scripts\check.ps1` rather than long one-liners

### Level 4: Quoting Problems (Windows Edge Case)
**Symptoms:**
- `python -c "..."` breaks only under Cline/Background Exec
- Output appears "escaped" or broken

**Stable Workaround:**
- Write diagnostics to a file: `scripts/diag.py` then run `.\.venv\Scripts\python.exe scripts\diag.py`
- Keep Cline commands "quote-light": Prefer `.\scripts\check.ps1` over long quoted commands

### Level 5: Performance / "Cline Becomes Imprecise"
**Symptoms:**
- Cline reads too much context
- Responses become sloppy, scope drifts

**Fix:**
- Expand `.clineignore` (caches, builds, data files, logs)
- Cut tasks smaller (max 1-3 files per iteration)
- Start new task and save status in memory-bank/activeContext.md / progress.md

### Diagnostic Prompt (Copy/Paste for Cline)
```
Plan Mode. Diagnose environment.
Goal: Figure out why tool runs/venv/terminal aren't working correctly.

Name the 3 most important checks you want to do first.
Assume the truth is .\scripts\check.ps1.
When suggesting commands: use only .venv\Scripts\python.exe and .\scripts\check.ps1.

Then create a fix plan in max 6 steps. No code changes.
```

## CI/CD
- **Platform**: GitHub Actions
- **Workflow**: `.github/workflows/ci.yml`
- **Triggers**: push, pull_request
- **Checks**: Same as local quality gate (ruff + pytest)

---

*Update this file when technical context changes.*
*Keep troubleshooting ladder current with discovered solutions.*
