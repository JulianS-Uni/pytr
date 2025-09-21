# pytr: TradeRepublic Terminal Library

pytr is a Python library and CLI tool for the private API of the Trade Republic online brokerage. It provides commands for portfolio management, document downloads, price alarms, and transaction exports.

**ALWAYS follow these instructions first and only fallback to additional search and context gathering if the information here is incomplete or found to be in error.**

## Working Effectively

### Bootstrap and Setup
Run these commands in sequence to set up a working development environment:

```bash
# Install uv package manager (if not available)
pip install uv

# Install all dependencies (takes ~0.5-4 seconds, depending on cache)
uv sync

# Verify setup by running tests (takes <1 second - 11 tests should pass)
uv run pytest
```

### Build and Package
```bash
# Build wheel and source distribution (takes ~1 second)
uv build

# The build produces:
# - dist/pytr-X.Y.Z.tar.gz (source distribution)
# - dist/pytr-X.Y.Z-py3-none-any.whl (wheel)
```

### Running the Application
```bash
# Show help and available commands (takes ~0.3 seconds)
uv run pytr --help

# Show specific command help
uv run pytr login --help
uv run pytr dl_docs --help

# Note: Most commands require TradeRepublic credentials
# The --version command will show version but may fail online version check without internet
```

## Validation and Quality Checks

**ALWAYS run these commands before committing changes. All must pass for CI to succeed:**

### Linting and Formatting (takes <0.1 seconds each)
```bash
# Format code - ALWAYS run this first
uv run ruff format

# Fix import issues
uv run ruff check --fix-only

# Check for remaining issues (must show "All checks passed!")
uv run ruff check
```

### Type Checking (takes ~3 seconds)
```bash
# Run type checker (must show "Success: no issues found")
uv run mypy .
```

### Testing (takes <1 second)
```bash
# Run all tests (must show "11 passed")
uv run pytest

# For verbose output
uv run pytest -v
```

## Manual Validation Requirements

**CRITICAL**: After making any changes to the core functionality, ALWAYS test these scenarios:

### Basic Application Functionality
```bash
# Test help system (takes ~0.3 seconds)
uv run pytr --help
uv run pytr help

# Test command discovery  
uv run pytr login --help
uv run pytr portfolio --help
uv run pytr dl_docs --help
```

### Version and Build Validation
```bash
# Test version (may show network error but should display version number)
uv run pytr --version

# Verify build still works (takes ~0.9 seconds)
uv build
```

### Combined Validation Pipeline
```bash
# Run all checks in sequence (takes ~3-4 seconds total)
uv run pytest && uv run ruff check && uv run mypy .
```

## Key Commands and Features

### Main Commands
- `login` - Check/create credentials, authenticate with TradeRepublic
- `dl_docs` - Download PDF documents and export transactions
- `portfolio` - Show current portfolio status
- `details` - Get details for specific ISIN
- `get_price_alarms` / `set_price_alarms` - Manage price alerts
- `export_transactions` - Export for Portfolio Performance
- `completion` - Shell tab completion

### Important Files and Locations
- `pytr/main.py` - Main CLI entry point and argument parsing
- `pytr/__main__.py` - Package execution entry point
- `pyproject.toml` - Project configuration and dependencies
- `tests/` - Test suite (11 tests covering event parsing and CSV formatting)
- `.github/workflows/publish-pypi.yml` - CI/CD pipeline

## Development Guidelines

### Code Style and Standards
- Uses **Ruff** for linting and formatting (line length: 120)
- Uses **Mypy** for type checking (Python 3.10+ target)
- Follows modern Python packaging with **uv** and **hatchling**
- All imports must be properly sorted (ruff handles this)

### Testing
- Pytest-based test suite with 11 tests
- Tests focus on event parsing and CSV formatting functionality
- All tests must pass - no broken tests are acceptable
- Tests run very quickly (<1 second total)

### Dependencies and Tools
- **uv** - Primary dependency manager and runner
- **ruff** - Linting and formatting
- **mypy** - Type checking  
- **pytest** - Testing framework
- **babel** - Internationalization
- **websockets** - Real-time API communication

### CI Requirements
The CI pipeline runs on Python 3.10, 3.11, 3.12, and 3.13 and checks:
1. `uv run pytest` - All tests must pass
2. `uv run ruff check` - No linting errors
3. `uv run ruff format --check --diff` - Code must be formatted
4. `uv run mypy .` - No type errors
5. README.md sync check with mksync

### README Maintenance
To update auto-generated content in README.md:
```bash
uvx mksync@0.1.4 -i README.md
```

## Common Tasks Reference

### Quick Development Workflow
```bash
# 1. Make your changes
# 2. Format and fix imports
uv run ruff format
uv run ruff check --fix-only

# 3. Run all validation (takes ~3-4 seconds total)
uv run pytest && uv run ruff check && uv run mypy .

# 4. Test basic functionality
uv run pytr --help

# 5. If all passes, commit your changes
```

### Repository Structure
```
pytr/                 # Main package directory
├── __init__.py       # Package initialization
├── __main__.py       # CLI entry point
├── main.py           # Argument parsing and command dispatch
├── account.py        # Authentication and login
├── api.py            # TradeRepublic API interface
├── portfolio.py      # Portfolio management
├── dl.py             # Document download functionality
├── alarms.py         # Price alarm management
├── transactions.py   # Transaction export
├── event.py          # Event data structures
├── timeline.py       # Timeline processing
├── translation.py    # Internationalization
├── utils.py          # Utility functions
└── locale/           # Translation files

tests/                # Test suite
├── test_event.py     # Event parsing tests
├── test_event_csv_formatter.py  # CSV formatting tests
├── test_parsing.py   # String parsing tests
└── sample_*.json     # Test data files
```

## Known Issues and Limitations

1. **Network Dependencies**: The `--version` command tries to check for updates online and will show an error without internet connectivity, but this doesn't affect core functionality.

2. **Authentication Required**: Most commands require valid TradeRepublic credentials. The application handles credential storage and device authentication.

3. **External API**: This library interacts with TradeRepublic's private API, so functionality depends on external service availability.

## Success Criteria

You have successfully set up and validated the environment when:
- [x] `uv sync` completes without errors
- [x] `uv run pytest` shows "11 passed" 
- [x] `uv run ruff check` shows "All checks passed!"
- [x] `uv run mypy .` shows "Success: no issues found"
- [x] `uv run pytr --help` displays the command interface
- [x] `uv build` creates wheel and source distribution