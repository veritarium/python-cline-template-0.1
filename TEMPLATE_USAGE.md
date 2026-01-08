# Template Usage - Quick Reference

Quick reference for using the python-cline-template.

## Quick Start Commands

### Bootstrap New Project (Full Setup)

```powershell
# Windows
.\scripts\bootstrap.ps1 -SetupVenv -NonInteractive

# Linux/macOS (requires pwsh)
pwsh ./scripts/bootstrap.ps1 -SetupVenv -NonInteractive
```

### Documentation Only (No venv)

```powershell
.\scripts\bootstrap.ps1
```

### Custom Project Name

```powershell
.\scripts\bootstrap.ps1 -ProjectName "my-project" -SetupVenv -NonInteractive
```

## Quality Gate Commands

```powershell
# Check everything (lint, format, tests)
.\scripts\check.ps1

# Auto-fix linting issues
.\scripts\fix.ps1

# Run tests only
.\.venv\Scripts\python.exe -m pytest tests/     # Windows
./.venv/bin/python -m pytest tests/             # Linux/macOS
```

## After Bootstrap - Next Steps

1. **Review changes**
   ```bash
   git status
   git diff
   ```

2. **Select Python interpreter in VS Code/code-server**
   - Open Command Palette (Ctrl+Shift+P)
   - "Python: Select Interpreter"
   - Choose `.venv/Scripts/python.exe` (Windows) or `.venv/bin/python` (Linux/macOS)

3. **Replace README.md with project-specific content**
   ```bash
   cp PROJECT_README_TEMPLATE.md README.md
   # Then edit README.md for your project
   ```

4. **Commit bootstrap changes**
   ```bash
   git add -A
   git commit -m "chore: bootstrap project"
   git push -u origin main
   ```

## What Gets Replaced During Bootstrap

- `__PROJECT_NAME__` in all documentation files → your project name
- Memory bank files updated with project context
- Documentation prepared for your project

## What Stays the Same

- Python package name: `devproj` (customize after bootstrap if needed)
- Core project structure
- Quality gate scripts
- Cline workflow rules in `.clinerules/`

## Development Workflow Quick Reference

```bash
# 1. Create feature branch
git checkout -b feature/my-feature

# 2. Verify baseline is green
.\scripts\check.ps1

# 3. Use Cline in Plan Mode
#    - Define goal, acceptance criteria, non-goals
#    - Create plan (max 10 steps)

# 4. Switch to Act Mode
#    - Implement one step at a time
#    - Run .\scripts\check.ps1 after each step
#    - Commit when green

# 5. Create PR
#    - Use template from .clinerules/91-pr-template.md
```

## Platform-Specific Notes

### Windows
- Use PowerShell (built-in)
- Python 3.12+ from python.org (not Microsoft Store)
- Virtual environment at `.venv\Scripts\`

### Linux/macOS
- Install `pwsh` for PowerShell scripts: `sudo apt-get install -y powershell`
- Or use Bash fallback: `bash ./scripts/check.sh`
- Virtual environment at `.venv/bin/`
- May need: `sudo apt-get install -y python3-venv`

## Common Issues

### Python opens Microsoft Store (Windows)
**Fix:** Disable App Execution Aliases
1. Settings → Apps → App Execution Aliases
2. Turn off `python.exe` and `python3.exe`

### PowerShell execution policy error (Windows)
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### Missing python3-venv (Linux)
```bash
sudo apt-get install -y python3-venv
```

### Git author identity unknown
```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

## Documentation Links

- **[README.md](README.md)** - Complete template documentation
- **[BOOTSTRAP.md](BOOTSTRAP.md)** - Detailed bootstrap process
- **[DEVELOPMENT.md](DEVELOPMENT.md)** - Development workflow and Cline integration
- **[PROJECT_README_TEMPLATE.md](PROJECT_README_TEMPLATE.md)** - Template for your project's README

## Template Maintenance

- **Repository:** https://github.com/veritarium/python-cline-template-0.1
- **Version:** See TEMPLATE_VERSION.txt
- **Changelog:** See CHANGELOG.md

---

For detailed information, see the main [README.md](README.md).
