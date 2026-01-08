# Project Brief: __PROJECT_NAME__

## Purpose
[Describe the main purpose of this project - what problem does it solve?]

Example: A Python application for processing data files and generating reports.

## Scope (What It Does)
[List the main features and capabilities]

- Feature 1: [Description]
- Feature 2: [Description]
- Feature 3: [Description]

## Non-Goals (What It Doesn't Do)
[List explicitly what is out of scope]

- Not intended for: [Example: production deployment without containerization]
- Does not include: [Example: user authentication]
- Not optimized for: [Example: real-time processing]

## Target Audience
[Who will use this project?]

Example: Data analysts and operations team members

## Definition of Done (Summary)
A feature/task is considered complete when:

- ✅ Feature is implemented according to acceptance criteria
- ✅ Tests pass (`.\scripts\check.ps1` is green)
- ✅ New behavior has tests (would fail if broken)
- ✅ Bug fixes include regression tests
- ✅ No side refactors or unrelated changes
- ✅ Documentation is updated if needed
- ✅ Changes committed in small, logical units

## Project Metadata

- **Created from:** python-cline-template v0.2.0
- **Primary Language:** Python 3.12+
- **Development Approach:** AI-assisted with Cline (Plan/Act workflow)
- **Quality Gate:** `.\scripts\check.ps1` (lint, format, test)
- **Repository:** [Add your repository URL]

## Key Constraints

- Python 3.12+ required
- Cross-platform support (Windows, Linux, macOS)
- Must maintain quality gate green before commits
- No new dependencies without approval
- Follow Cline workflow (see DEVELOPMENT.md)

## Success Criteria

[How do you know when the project is successful?]

Example:
- Processes at least 1000 records per minute
- Generates reports in under 5 seconds
- Zero data corruption incidents
- Test coverage above 80%

---

*Note: Update this file as the project evolves. Keep it concise and current.*
