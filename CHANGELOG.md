# Changelog

All notable changes to the python-cline-template will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.2.0] - 2026-01-08

### Added
- **PROJECT_README_TEMPLATE.md** - Template for project README after instantiation
- **GitHub Actions CI workflow** (.github/workflows/ci.yml)
  - Cross-platform testing (Ubuntu & Windows)
  - Quality gate automation (lint, format, test)
  - Test artifact uploads
- **LICENSE file** - MIT License
- Comprehensive documentation restructure for better clarity

### Changed
- **README.md** - Completely restructured as template-focused documentation
  - Clear explanation of what the template is
  - Quick start guide for creating new projects
  - Platform-specific setup instructions
  - Better organized sections with emojis for visual clarity
- **TEMPLATE_USAGE.md** - Simplified to quick reference guide
  - Removed redundant content now in README.md
  - Focus on commands and common issues
  - Better troubleshooting section
- **DEVELOPMENT.md** - Translated from German to English
  - Maintained all original content and structure
  - Added cross-platform notes
  - Clearer formatting and sections
- **Documentation hierarchy** - Clear separation of concerns
  - README.md: For template users
  - PROJECT_README_TEMPLATE.md: For project instances
  - TEMPLATE_USAGE.md: Quick reference
  - DEVELOPMENT.md: Workflow guide
  - BOOTSTRAP.md: Technical details

### Improved
- Documentation clarity and organization
- Cross-platform instructions (Windows, Linux, macOS)
- Template instantiation workflow
- Onboarding experience for new users

### Fixed
- Documentation redundancy and confusion
- Unclear relationship between README.md and project documentation
- Missing CI/CD configuration (now included)
- Missing LICENSE file (now included)
- Language inconsistency (now all English)

## [0.1.0] - 2026-01-XX

### Added
- Initial template: Windows + VS Code + Python + Cline workflow
- Quality gate scripts:
  - `scripts/check.ps1` - PowerShell quality gate
  - `scripts/check.sh` - Bash quality gate (Linux fallback)
  - `scripts/fix.ps1` - Auto-fix script
- Bootstrap script (`scripts/bootstrap.ps1`) for template initialization
  - Documentation placeholder replacement
  - Optional venv setup
  - Dependency installation
- Cline workflow integration:
  - `.clinerules/` directory with workflow rules
  - Plan/Act mode guidelines
  - Commit discipline templates
  - PR templates
- Memory bank structure for AI context:
  - `memory-bank/activeContext.md`
  - `memory-bank/productContext.md`
  - `memory-bank/progress.md`
  - `memory-bank/projectbrief.md`
  - `memory-bank/systemPatterns.md`
  - `memory-bank/techContext.md`
- Project structure:
  - `devproj/` - Example Python package
  - `tests/` - Example test suite
  - `pyproject.toml` - Tool configuration
  - `requirements.txt` and `requirements-dev.txt`
- Documentation:
  - README.md
  - BOOTSTRAP.md
  - DEVELOPMENT.md (German)
  - TEMPLATE_USAGE.md
- Configuration files:
  - `.clineignore` - Files to exclude from Cline
  - `.gitignore` - Python-specific gitignore
  - `.editorconfig` - Editor configuration

### Technical Details
- Python 3.12+ support
- Cross-platform PowerShell scripts
- Ruff for linting and formatting
- pytest for testing
- Virtual environment isolation

## Upgrade Guide

### From 0.1 to 0.2

If you've already instantiated a project from v0.1:

1. **Optional: Update documentation structure**
   - Copy `PROJECT_README_TEMPLATE.md` to your project
   - Review and adapt your README.md if needed

2. **Add GitHub Actions CI (recommended)**
   - Copy `.github/workflows/ci.yml` to your project
   - Adjust if you have custom requirements

3. **Add LICENSE file**
   - Copy `LICENSE` and update copyright holder if needed

4. **Update DEVELOPMENT.md** (if using German version)
   - Replace with English version from v0.2

5. **No breaking changes** - All scripts and workflows remain compatible

---

## Version History

- **0.2.0** (2026-01-08) - Documentation restructure, CI/CD, LICENSE
- **0.1.0** (2026-01-XX) - Initial release

## Contributing

See the main [README.md](README.md) for contribution guidelines.

## Template Maintenance

This template is actively maintained. For issues or suggestions:
- GitHub Issues: https://github.com/veritarium/python-cline-template-0.1/issues
- Pull Requests: https://github.com/veritarium/python-cline-template-0.1/pulls
