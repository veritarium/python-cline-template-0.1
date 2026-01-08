# Python Cline Template

A production-ready Python project template with modern development tools, quality gates, and AI-assisted workflow integration.

## 🎯 What is this?

This is a **template repository** for creating Python projects with:

- ✅ **Quality gates** - Single-command verification (`.\scripts\check.ps1`)
- ✅ **Virtual environment** - Isolated dependencies per project
- ✅ **Testing infrastructure** - pytest with example tests
- ✅ **Code quality tools** - ruff for linting and formatting
- ✅ **Cline workflow** - Structured Plan/Act mode development
- ✅ **Cross-platform** - Windows, Linux, and macOS support
- ✅ **Bootstrap automation** - One command to set up new projects

## 🚀 Quick Start

### Creating a New Project from this Template

1. **Use this template on GitHub**
   - Click the "Use this template" button on GitHub
   - Or clone and create a new repository:
     ```bash
     git clone https://github.com/veritarium/python-cline-template-0.1.git my-project
     cd my-project
     rm -rf .git
     git init
     ```

2. **Bootstrap your project** (replaces placeholders and sets up environment)
   ```powershell
   # Windows
   .\scripts\bootstrap.ps1 -SetupVenv -NonInteractive
   
   # Linux/macOS (requires pwsh)
   pwsh ./scripts/bootstrap.ps1 -SetupVenv -NonInteractive
   ```

3. **Verify everything works**
   ```powershell
   .\scripts\check.ps1
   ```

4. **Commit the bootstrapped project**
   ```bash
   git add -A
   git commit -m "chore: bootstrap project"
   git push -u origin main
   ```

**That's it!** Your project is ready for development.

## 📋 What the Bootstrap Does

The bootstrap script (`scripts/bootstrap.ps1`) performs these tasks:

1. **Replaces placeholders** in documentation files:
   - `__PROJECT_NAME__` → your project name (defaults to folder name)
   - Updates memory-bank files with project context

2. **Sets up Python environment** (when `-SetupVenv` is used):
   - Creates `.venv` virtual environment
   - Installs development dependencies (ruff, pytest)
   - Runs quality gate to verify setup

3. **Prepares project for development**:
   - Updates documentation
   - Configures Cline workflow rules
   - Sets up quality gates

## 🛠️ What's Included

### Project Structure

```
your-project/
├── .clinerules/              # AI workflow rules (Plan/Act mode)
├── .venv/                    # Virtual environment (created by bootstrap)
├── devproj/                  # Main Python package
│   ├── __init__.py
│   └── main.py              # Example module with tests
├── tests/                    # Test suite
│   ├── test_greeting.py
│   └── test_smoke.py
├── scripts/
│   ├── bootstrap.ps1        # Project initialization
│   ├── check.ps1            # Quality gate (lint + format + test)
│   ├── check.sh             # Bash version (Linux fallback)
│   └── fix.ps1              # Auto-fix linting issues
├── memory-bank/             # Project context for AI assistants
├── pyproject.toml           # Tool configuration (ruff, pytest)
├── requirements.txt         # Runtime dependencies
├── requirements-dev.txt     # Development dependencies
├── DEVELOPMENT.md           # Development workflow guide
├── BOOTSTRAP.md            # Detailed bootstrap documentation
└── PROJECT_README_TEMPLATE.md  # Becomes README.md after bootstrap
```

### Quality Gates

**Single command to verify everything:**
```powershell
.\scripts\check.ps1
```

This runs:
- ✅ `ruff check .` - Linting
- ✅ `ruff format --check .` - Format verification
- ✅ `pytest` - All tests

### Cline Workflow Integration

The template includes `.clinerules/` with structured workflows:

- **Plan Mode** - Create detailed plans before implementation
- **Act Mode** - Execute one step at a time
- **Quality gates** - Mandatory checks after each step
- **Commit discipline** - Small, focused commits
- **PR templates** - Structured pull request documentation

See [DEVELOPMENT.md](DEVELOPMENT.md) for the complete workflow guide.

## 📖 Detailed Guides

### For Windows Users

**Prerequisites:**
- Python 3.12+ (from python.org, not Microsoft Store)
- Git for Windows
- PowerShell (built-in)

**Setup:**
```powershell
cd C:\dev\projects
git clone https://github.com/veritarium/python-cline-template-0.1.git my-project
cd my-project

# Bootstrap with venv setup
.\scripts\bootstrap.ps1 -SetupVenv -NonInteractive

# Verify
.\scripts\check.ps1

# Commit
git add -A
git commit -m "chore: bootstrap project"
```

### For Linux/macOS Users

**Prerequisites:**
- Python 3.12+ (`python3`, `python3-venv`, `python3-pip`)
- Git
- PowerShell Core (`pwsh`) - recommended
  ```bash
  # Ubuntu/Debian
  sudo apt-get install -y powershell
  ```

**Setup:**
```bash
cd ~/dev/projects
git clone https://github.com/veritarium/python-cline-template-0.1.git my-project
cd my-project

# Bootstrap with venv setup
pwsh ./scripts/bootstrap.ps1 -SetupVenv -NonInteractive

# Verify (use pwsh or bash version)
pwsh ./scripts/check.ps1
# Or: bash ./scripts/check.sh

# Commit
git add -A
git commit -m "chore: bootstrap project"
```

### Bootstrap Options

```powershell
# Full setup (recommended for new projects)
.\scripts\bootstrap.ps1 -SetupVenv -NonInteractive

# Custom project name (instead of folder name)
.\scripts\bootstrap.ps1 -ProjectName "my-awesome-project" -SetupVenv

# Documentation only (no venv setup)
.\scripts\bootstrap.ps1

# Interactive mode (with confirmation prompts)
.\scripts\bootstrap.ps1 -SetupVenv
```

## 🔧 Development Workflow

Once bootstrapped, follow this workflow:

1. **Create feature branch**
   ```bash
   git checkout -b feature/my-feature
   ```

2. **Verify baseline is green**
   ```powershell
   .\scripts\check.ps1
   ```

3. **Use Cline in Plan Mode**
   - Define goal, acceptance criteria, and non-goals
   - Create a plan (max 10 steps)
   - Get approval before implementation

4. **Switch to Act Mode**
   - Implement ONE plan step at a time
   - Run quality gate after each step
   - Commit when green

5. **Create PR**
   - Use PR template from `.clinerules/91-pr-template.md`
   - Ensure all checks pass

See [DEVELOPMENT.md](DEVELOPMENT.md) for complete guidelines.

## 🎨 Customization

After bootstrapping, customize these files for your project:

- **README.md** - Replace with PROJECT_README_TEMPLATE.md content
- **devproj/** - Rename package or add your code
- **tests/** - Add your test cases
- **requirements.txt** - Add your runtime dependencies
- **pyproject.toml** - Adjust project metadata

## 📚 Template Documentation

- **[TEMPLATE_USAGE.md](TEMPLATE_USAGE.md)** - Quick reference for template usage
- **[BOOTSTRAP.md](BOOTSTRAP.md)** - Detailed bootstrap process documentation
- **[DEVELOPMENT.md](DEVELOPMENT.md)** - Development workflow and Cline integration
- **[CHANGELOG.md](CHANGELOG.md)** - Template version history

## 🤝 Contributing to the Template

This template is maintained to provide the best Python development experience with Cline.

To contribute improvements:

1. Fork this template repository
2. Create a feature branch
3. Follow the development workflow
4. Ensure `.\scripts\check.ps1` passes
5. Submit a PR with clear description

## 📦 What's Different from Standard Python Projects?

- **AI-first workflow** - Optimized for Cline-assisted development
- **Quality gate focus** - Single source of truth for code quality
- **Bootstrap automation** - Zero-friction project setup
- **Workflow discipline** - Structured Plan/Act development process
- **Cross-platform scripts** - PowerShell that works everywhere
- **Memory bank** - Project context for AI assistants

## ⚖️ License

MIT License - See LICENSE file

## 🙏 Acknowledgments

Built for modern Python development with:
- **pytest** - Testing framework
- **ruff** - Fast Python linter and formatter
- **Cline** - AI-assisted development workflow
- **PowerShell** - Cross-platform automation

---

**Ready to start your next Python project?** Click "Use this template" and run the bootstrap! 🚀
