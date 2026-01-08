# System Patterns

## Repository Layout
```
__PROJECT_NAME__/
├── devproj/              # Main Python package
│   ├── __init__.py       # Package initialization
│   └── main.py           # Core module
├── tests/                # Test suite
│   ├── test_*.py         # Test files
├── scripts/              # Development scripts
│   ├── bootstrap.ps1     # Project setup
│   ├── check.ps1         # Quality gate
│   └── fix.ps1           # Auto-fix issues
├── .clinerules/          # Cline AI workflow rules
├── memory-bank/          # Project context for AI
├── .venv/                # Virtual environment (gitignored)
└── pyproject.toml        # Project configuration
```

## Where Core Logic Lives
- **Main module**: `devproj/main.py`
- **Package init**: `devproj/__init__.py`
- [Add additional modules as your project grows]

## Where IO/CLI Lives
- **CLI entry point**: `devproj/main.py:main()` (or `devproj/cli.py` if separated)
- **Console script**: Defined in `pyproject.toml` under `[project.scripts]`

## Error Handling Conventions
- Use standard Python exceptions
- Raise `ValueError` for invalid arguments
- Raise `RuntimeError` for operational errors
- Document exceptions in docstrings
- Use pytest for testing error cases

Example:
```python
def process(value: int) -> int:
    """Process a value.
    
    Raises:
        ValueError: If value is negative
    """
    if value < 0:
        raise ValueError(f"Value cannot be negative: {value}")
    return value * 2
```

## Public APIs That Must Not Break
- [List functions/classes that are part of the public API]
- [These should have stable interfaces]

Example:
- `devproj.main.greet(name, count)` - Main greeting function
- `devproj.main.add(a, b)` - Addition function

## Testing Patterns
- Test files in `tests/` directory
- Naming: `test_*.py`
- Use pytest fixtures for setup
- Test both success and error cases

Example:
```python
def test_function_success():
    """Test successful execution."""
    assert function("input") == "expected"

def test_function_error():
    """Test error handling."""
    with pytest.raises(ValueError):
        function(invalid_input)
```

## Configuration Patterns
- Tool config in `pyproject.toml`
- Runtime config via environment variables or config files
- Secrets via `.env` files (gitignored)

## Import Patterns
```python
# Absolute imports preferred
from devproj.main import function

# Avoid relative imports in tests
from devproj.main import greet  # Good
from ..main import greet        # Avoid
```

---

*Update this file when architectural patterns change.*
*Keep it current to help AI assistants understand your codebase.*
