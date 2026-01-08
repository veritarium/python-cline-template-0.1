# __PROJECT_NAME__

A Python project with modern development tools, testing, and code quality gates.

## Features

- **Virtual environment management** - Isolated Python dependencies
- **Automated testing** with pytest
- **Code formatting and linting** with ruff
- **Quality gate script** - Single command to verify code quality
- **Cline workflow integration** - Structured Plan/Act development process

## Getting Started

### Prerequisites

- Python 3.12 or higher
- Git
- PowerShell (Windows) or pwsh (Linux/macOS)

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd __PROJECT_NAME__
   ```

2. **Set up the development environment**
   ```powershell
   # Windows
   .\scripts\bootstrap.ps1 -SetupVenv -NonInteractive
   
   # Linux/macOS (with pwsh installed)
   pwsh ./scripts/bootstrap.ps1 -SetupVenv -NonInteractive
   ```

3. **Verify the setup**
   ```powershell
   .\scripts\check.ps1
   ```

### Development

#### Running Tests

```bash
# Using venv Python directly (cross-platform)
.\.venv\Scripts\python.exe -m pytest tests/     # Windows
./.venv/bin/python -m pytest tests/             # Linux/macOS

# Or with activated environment
pytest tests/
```

#### Code Quality

Run the quality gate (linting, formatting, tests):
```powershell
.\scripts\check.ps1
```

Fix auto-fixable issues:
```powershell
.\scripts\fix.ps1
```

## Project Structure

```
__PROJECT_NAME__/
├── .venv/                    # Python virtual environment (gitignored)
├── devproj/                  # Main package
│   ├── __init__.py          # Package initialization
│   └── main.py              # Main module
├── tests/                    # Test suite
│   ├── test_greeting.py     # Example tests
│   └── test_smoke.py        # Smoke tests
├── scripts/                  # Development scripts
│   ├── bootstrap.ps1        # Project setup
│   ├── check.ps1            # Quality gate
│   └── fix.ps1              # Auto-fix issues
├── .clinerules/             # Cline AI workflow rules
├── memory-bank/             # Project context for AI assistants
├── pyproject.toml           # Project configuration
├── requirements.txt         # Runtime dependencies
├── requirements-dev.txt     # Development dependencies
├── DEVELOPMENT.md           # Development workflow guide
└── README.md                # This file
```

## Development Workflow

This project follows a structured development workflow optimized for AI-assisted development:

1. **Create a feature branch**
   ```bash
   git checkout -b feature/<name>
   ```

2. **Verify baseline** (before making changes)
   ```powershell
   .\scripts\check.ps1
   ```

3. **Plan Mode** - Define goals and create a plan (when using Cline)
   - Define acceptance criteria
   - Identify affected files
   - Create step-by-step plan

4. **Act Mode** - Implement one step at a time
   - Make focused changes
   - Run quality gate after each step
   - Commit small, logical units

5. **Quality gate** - Always green before committing
   ```powershell
   .\scripts\check.ps1
   ```

6. **Commit discipline** - Small, clear commits
   ```bash
   git commit -m "feat(core): add new feature"
   git commit -m "fix(cli): resolve issue with parameter"
   git commit -m "test: add regression test for bug"
   ```

For detailed workflow guidelines, see [DEVELOPMENT.md](DEVELOPMENT.md).

## Usage

[Add project-specific usage instructions here]

Example:
```python
from devproj.main import greet

# Use your application
result = greet("World", 5)
print(result)
```

## Testing

Run all tests:
```bash
pytest
```

Run with verbose output:
```bash
pytest -v
```

Run specific test file:
```bash
pytest tests/test_greeting.py
```

Run with coverage:
```bash
pytest --cov=devproj tests/
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Follow the development workflow (see above)
4. Ensure quality gate passes (`.\scripts\check.ps1`)
5. Commit your changes with clear messages
6. Push to the branch (`git push origin feature/amazing-feature`)
7. Open a Pull Request

## License

[Specify your license here - MIT suggested]

## Acknowledgments

- Built with [python-cline-template](https://github.com/veritarium/python-cline-template-0.1)
- Configured for optimal development with Cline AI assistant
