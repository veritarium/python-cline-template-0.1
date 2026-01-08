# Contributing to python-cline-template

Thank you for your interest in contributing to the python-cline-template! This document provides guidelines for contributing to the template itself.

> **Note:** This guide is for contributing to the **template repository**. If you're working on a project created from this template, adapt these guidelines for your specific project.

## Table of Contents

- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Contribution Types](#contribution-types)
- [Coding Standards](#coding-standards)
- [Commit Guidelines](#commit-guidelines)
- [Pull Request Process](#pull-request-process)
- [Testing](#testing)

## Getting Started

1. **Fork the repository**
   ```bash
   # Fork via GitHub UI, then clone your fork
   git clone https://github.com/YOUR-USERNAME/python-cline-template-0.1.git
   cd python-cline-template-0.1
   ```

2. **Set up the development environment**
   ```powershell
   # Bootstrap the template
   .\scripts\bootstrap.ps1 -SetupVenv -NonInteractive
   
   # Verify setup
   .\scripts\check.ps1
   ```

3. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   # or
   git checkout -b fix/your-fix-name
   ```

## Development Workflow

This project follows the **Cline workflow** as described in [DEVELOPMENT.md](DEVELOPMENT.md):

### Plan Mode First

Before making changes:

1. **Define your goal clearly**
   - What problem are you solving?
   - What are the acceptance criteria?
   - What is explicitly out of scope?

2. **Create a plan** (max 10 steps)
   - List affected files
   - Specify tests/checks for each step

3. **Get feedback** (optional but recommended)
   - Open an issue to discuss major changes
   - Share your plan for review

### Act Mode: Implement

1. **Baseline check**
   ```powershell
   .\scripts\check.ps1  # Must be green before you start
   ```

2. **Implement ONE step at a time**
   - Make focused changes
   - Run quality gate after each step
   - Commit when green

3. **Quality gate discipline**
   ```powershell
   .\scripts\check.ps1  # After every change
   ```

## Contribution Types

### Documentation Improvements

- Fix typos, unclear instructions, or outdated information
- Add missing sections or examples
- Improve cross-platform instructions
- Files: `*.md` files

### Script Enhancements

- Improve bootstrap.ps1, check.ps1, or fix.ps1
- Add new useful scripts
- Better error handling or cross-platform support
- Files: `scripts/*.ps1`, `scripts/*.sh`

### Workflow Improvements

- Better Cline rules or templates
- New VSCode tasks or configurations
- Improved quality gates
- Files: `.clinerules/*`, `.vscode/*`

### Template Features

- New useful patterns for Python projects
- Better demo code examples
- Additional tooling integration
- Files: `devproj/*`, `tests/*`, configuration files

## Coding Standards

### Python Code

- **Style:** Follow PEP 8 (enforced by ruff)
- **Type hints:** Use type annotations for function signatures
- **Docstrings:** Use Google-style docstrings
- **Testing:** Every feature needs tests

### PowerShell Scripts

- **Cross-platform:** Test on both Windows and Linux (pwsh)
- **Error handling:** Use `$ErrorActionPreference = "Stop"`
- **Output:** Clear, colored messages for user feedback
- **Parameters:** Document all parameters with comments

### Documentation

- **Markdown:** Follow standard Markdown conventions
- **Structure:** Use clear headings and sections
- **Examples:** Provide working code examples
- **Platform-specific:** Mark Windows/Linux/macOS differences clearly

## Commit Guidelines

### Commit Message Format

```
<type>(<scope>): <subject>

[optional body]

[optional footer]
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `chore`: Maintenance tasks
- `test`: Test additions or modifications
- `refactor`: Code refactoring

**Examples:**
```
feat(scripts): add option to skip tests in check.ps1
fix(bootstrap): handle spaces in project names correctly
docs(readme): clarify Windows setup instructions
chore(deps): update ruff to 0.2.0
test(greeting): add edge case for empty string
```

### Commit Discipline

- **Small commits:** One logical change per commit
- **Meaningful messages:** Clear description of what and why
- **No WIP commits:** All commits should be complete and working
- **Quality gate:** Every commit should pass `.\scripts\check.ps1`

## Pull Request Process

### Before Submitting

1. **Quality gate is green**
   ```powershell
   .\scripts\check.ps1  # Must pass
   ```

2. **All tests pass**
   ```powershell
   pytest tests/ -v
   ```

3. **Documentation is updated**
   - Update README.md if needed
   - Update CHANGELOG.md
   - Add/update relevant docs

4. **Commits are clean**
   - Small, logical commits
   - Clear commit messages
   - No "fix typo" or "WIP" commits

### PR Description Template

```markdown
## Motivation
[Why is this change needed? What problem does it solve?]

## Changes
- [Specific change 1]
- [Specific change 2]
- [Specific change 3]

## Testing
- [ ] `.\scripts\check.ps1` passes
- [ ] Tested on Windows
- [ ] Tested on Linux (if platform-specific)
- [ ] New features have tests
- [ ] Existing tests still pass

## Documentation
- [ ] README.md updated (if needed)
- [ ] CHANGELOG.md updated
- [ ] Comments added for complex logic

## Breaking Changes
[List any breaking changes, or write "None"]

## Screenshots/Examples
[If applicable, add screenshots or example output]
```

### Review Process

1. **Maintainer review** - A maintainer will review your PR
2. **Feedback** - Address any requested changes
3. **CI checks** - GitHub Actions must pass
4. **Approval** - Once approved, a maintainer will merge

### What We Look For

✅ **Good:**
- Clear problem statement
- Small, focused changes
- Tests included
- Documentation updated
- Quality gate passes
- Cross-platform compatibility

❌ **Avoid:**
- Large, unfocused PRs
- Missing tests
- Breaking changes without discussion
- Reformatting unrelated code
- Mixing multiple unrelated changes

## Testing

### Running Tests Locally

```powershell
# Full quality gate
.\scripts\check.ps1

# Just tests
pytest tests/ -v

# With coverage
pytest --cov=devproj tests/

# Specific test file
pytest tests/test_greeting.py -v
```

### Test Requirements

- **Coverage:** Aim for high test coverage (80%+)
- **Meaningful tests:** Tests should fail if functionality breaks
- **Fast tests:** Keep tests quick (< 5 seconds total)
- **No external dependencies:** Tests should not require network/external services

### Writing Tests

```python
# tests/test_example.py
import pytest
from devproj.module import function

def test_function_basic():
    """Test basic functionality."""
    result = function("input")
    assert result == "expected"

def test_function_edge_case():
    """Test edge case."""
    with pytest.raises(ValueError):
        function(invalid_input)
```

## Questions or Problems?

- **Template issues:** Open a GitHub issue
- **Usage questions:** Check TEMPLATE_USAGE.md or open a discussion
- **Security issues:** Email maintainers (see README.md)

## Code of Conduct

- Be respectful and constructive
- Focus on the code, not the person
- Welcome newcomers
- Follow the Golden Rule

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

Thank you for contributing to python-cline-template! 🎉
