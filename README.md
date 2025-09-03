# Ruff Complete Reference Card & Cheat Sheet

## Overview

Ruff is an extremely fast Python linter and code formatter written in Rust. It combines the functionality of multiple tools like Flake8, Black, isort, and more into a single, lightning-fast tool.

## Installation & Setup

### Installation

```bash
# Via pip
pip install ruff

# Via conda
conda install -c conda-forge ruff

# Via homebrew (macOS)
brew install ruff
```

### Basic Usage

```bash
# Check files for issues
ruff check .

# Check and auto-fix issues
ruff check --fix .

# Format code
ruff format .

# Check specific files
ruff check file1.py file2.py

# Show available rules
ruff linter
```

## Command Line Interface

### Core Commands

|Command      |Description   |Example          |
|-------------|--------------|-----------------|
|`ruff check` |Run linter    |`ruff check src/`|
|`ruff format`|Format code   |`ruff format .`  |
|`ruff rule`  |Show rule info|`ruff rule F401` |
|`ruff linter`|List all rules|`ruff linter`    |
|`--version`  |Show version  |`ruff --version` |
|`--help`     |Show help     |`ruff --help`    |

### Check Command Options

|Option              |Description          |Example                                         |
|--------------------|---------------------|------------------------------------------------|
|`--fix`             |Auto-fix violations  |`ruff check --fix .`                            |
|`--diff`            |Show diffs for fixes |`ruff check --diff .`                           |
|`--watch`           |Watch for changes    |`ruff check --watch .`                          |
|`--select`          |Enable specific rules|`ruff check --select E,F .`                     |
|`--ignore`          |Ignore specific rules|`ruff check --ignore E203 .`                    |
|`--exclude`         |Exclude files/dirs   |`ruff check --exclude tests/ .`                 |
|`--extend-exclude`  |Additional exclusions|`ruff check --extend-exclude docs/ .`           |
|`--per-file-ignores`|File-specific ignores|`ruff check --per-file-ignores "tests/*:F401" .`|
|`--show-files`      |Show processed files |`ruff check --show-files .`                     |
|`--show-settings`   |Show configuration   |`ruff check --show-settings .`                  |
|`--statistics`      |Show violation stats |`ruff check --statistics .`                     |
|`--output-format`   |Output format        |`ruff check --output-format json .`             |

### Format Command Options

|Option            |Description               |Example                                            |
|------------------|--------------------------|---------------------------------------------------|
|`--diff`          |Show formatting diffs     |`ruff format --diff .`                             |
|`--check`         |Check if formatting needed|`ruff format --check .`                            |
|`--stdin-filename`|Format stdin with filename|`echo "code" | ruff format --stdin-filename foo.py`|
|`--line-length`   |Set line length           |`ruff format --line-length 120 .`                  |

## Configuration

### pyproject.toml Configuration

```toml
[tool.ruff]
# Basic settings
line-length = 88
indent-width = 4
target-version = "py38"

# File discovery
include = ["*.py", "*.pyi", "**/pyproject.toml"]
exclude = [
    ".bzr",
    ".direnv",
    ".eggs",
    ".git",
    ".git-rewrite",
    ".hg",
    ".mypy_cache",
    ".nox",
    ".pants.d",
    ".pytype",
    ".ruff_cache",
    ".svn",
    ".tox",
    ".venv",
    "__pypackages__",
    "_build",
    "buck-out",
    "build",
    "dist",
    "node_modules",
    "venv",
]

# Linting
[tool.ruff.lint]
select = ["E", "F", "UP", "B", "SIM", "I"]
ignore = ["E501", "B008", "B006"]
fixable = ["ALL"]
unfixable = []
dummy-variable-rgx = "^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$"

# Per-file configuration
[tool.ruff.lint.per-file-ignores]
"__init__.py" = ["F401"]
"tests/**/*" = ["S101", "D"]

# Formatter
[tool.ruff.format]
quote-style = "double"
indent-style = "space"
skip-source-first-line = false
line-ending = "auto"

# Plugin configurations
[tool.ruff.lint.isort]
known-first-party = ["myproject"]
split-on-trailing-comma = true

[tool.ruff.lint.mccabe]
max-complexity = 10

[tool.ruff.lint.pydocstyle]
convention = "google"
```

### Alternative Configuration Files

```toml
# ruff.toml
line-length = 100
select = ["E", "F"]
ignore = ["E203"]
```

```ini
# .ruff.toml or setup.cfg
[tool:ruff]
line-length = 88
select = E,F,UP,B
```

## Rule Categories & Examples

### Error Prevention (E, F)

```python
# E902: IOError
# F401: Unused import
import unused_module  # F401

# F841: Unused variable
def func():
    x = 1  # F841 - unused variable
    return 2
```

### Code Style (W, C90)

```python
# W291: Trailing whitespace
def func():
    return True   # W291

# C901: Complex function
def complex_func(x):  # C901 if too complex
    if x > 1:
        if x > 2:
            # ... many nested conditions
            pass
```

### Pyflakes (F)

```python
# F405: Star import may be undefined
from module import *  # F405

# F811: Redefined name
def func(): pass
def func(): pass  # F811

# F632: Use == for equality
if x is "string":  # F632
    pass
```

### pycodestyle (E, W)

```python
# E225: Missing whitespace around operator
x=1+2  # E225

# E302: Expected 2 blank lines
class MyClass:
    pass
class AnotherClass:  # E302
    pass

# W503: Line break before binary operator
x = (1 +  # W503
     2)
```

### pyupgrade (UP)

```python
# UP006: Use list instead of List
from typing import List
def func() -> List[str]:  # UP006
    return []

# UP007: Use X | Y for unions
from typing import Union
def func(x: Union[str, int]):  # UP007
    return x
```

### flake8-bugbear (B)

```python
# B006: Mutable default argument
def func(items=[]):  # B006
    return items

# B008: Do not perform function call in argument defaults
import time
def func(timestamp=time.time()):  # B008
    return timestamp
```

### isort (I)

```python
# I001: Imports not sorted
import sys
import os  # I001 - should come before sys

# Correct order:
import os
import sys
```

## Complete Rule Reference Table

|Code                  |Category   |Description                                                  |Auto-fix|
|----------------------|-----------|-------------------------------------------------------------|--------|
|**Pyflakes (F)**                                                                                      ||||
|F401                  |Import     |Unused import                                                |✅       |
|F402                  |Import     |Import shadowed by loop var                                  |❌       |
|F403                  |Import     |Star import used                                             |❌       |
|F404                  |Import     |Late future import                                           |❌       |
|F405                  |Import     |Name may be undefined (star import)                          |❌       |
|F811                  |Variable   |Redefined while unused                                       |❌       |
|F821                  |Variable   |Undefined name                                               |❌       |
|F822                  |Variable   |Undefined name in **all**                                    |❌       |
|F831                  |Variable   |Duplicate argument name                                      |❌       |
|F841                  |Variable   |Local variable assigned but never used                       |✅       |
|**pycodestyle (E, W)**                                                                                ||||
|E101                  |Indentation|Indentation contains mixed spaces/tabs                       |✅       |
|E111                  |Indentation|Indentation not multiple of 4                                |✅       |
|E112                  |Indentation|Expected indented block                                      |❌       |
|E113                  |Indentation|Unexpected indentation                                       |❌       |
|E201                  |Whitespace |Whitespace after ‘(’                                         |✅       |
|E202                  |Whitespace |Whitespace before ‘)’                                        |✅       |
|E203                  |Whitespace |Whitespace before ‘:’                                        |✅       |
|E225                  |Whitespace |Missing whitespace around operator                           |✅       |
|E231                  |Whitespace |Missing whitespace after ‘,’                                 |✅       |
|E251                  |Whitespace |Unexpected spaces around =                                   |✅       |
|E302                  |Blank Lines|Expected 2 blank lines                                       |✅       |
|E303                  |Blank Lines|Too many blank lines                                         |✅       |
|E501                  |Line Length|Line too long                                                |❌       |
|E701                  |Statements |Multiple statements on one line                              |✅       |
|E702                  |Statements |Multiple statements on one line (semicolon)                  |✅       |
|E703                  |Statements |Statement ends with semicolon                                |✅       |
|W291                  |Whitespace |Trailing whitespace                                          |✅       |
|W292                  |Whitespace |No newline at end of file                                    |✅       |
|W293                  |Whitespace |Blank line contains whitespace                               |✅       |
|**pyupgrade (UP)**                                                                                    ||||
|UP001                 |Syntax     |Use raw strings for regex                                    |✅       |
|UP006                 |Typing     |Use list instead of List                                     |✅       |
|UP007                 |Typing     |Use X | Y for unions                                         |✅       |
|UP008                 |Syntax     |Use super() instead of super(**class**, self)                |✅       |
|UP009                 |Syntax     |UTF-8 encoding declaration is unnecessary                    |✅       |
|UP010                 |Syntax     |Unnecessary **future** import                                |✅       |
|UP011                 |Syntax     |Unnecessary parentheses to functools.lru_cache               |✅       |
|UP012                 |Syntax     |Unnecessary call to encode as UTF-8                          |✅       |
|UP013                 |Syntax     |Convert TypedDict functional to class syntax                 |✅       |
|UP014                 |Syntax     |Convert NamedTuple functional to class syntax                |✅       |
|UP015                 |Syntax     |Unnecessary open mode parameters                             |✅       |
|**flake8-bugbear (B)**                                                                                ||||
|B002                  |Logic      |Python does not support the unary prefix increment           |❌       |
|B003                  |Logic      |Assigning to os.environ doesn’t clear environment            |❌       |
|B004                  |Logic      |Using hasattr(x, “**call**”) to test if x is callable        |❌       |
|B005                  |Logic      |Using .strip() with multi-character strings                  |❌       |
|B006                  |Logic      |Do not use mutable data structures for argument defaults     |❌       |
|B007                  |Logic      |Loop control variable not used within loop body              |❌       |
|B008                  |Logic      |Do not perform function call in argument defaults            |❌       |
|B009                  |Logic      |Do not call getattr(x, ‘attr’) with a constant attribute     |✅       |
|B010                  |Logic      |Do not call setattr(x, ‘attr’, val) with a constant attribute|✅       |

## Advanced Usage & Techniques

### Inline Configuration

```python
# Disable specific rules for a line
import unused_module  # noqa: F401

# Disable multiple rules
x = lambda: None  # noqa: E731,F841

# Disable for entire file
# ruff: noqa

# Enable specific rule for file
# ruff: noqa: F401
```

### Pre-commit Integration

```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.1.0
    hooks:
      - id: ruff
        args: [--fix, --exit-non-zero-on-fix]
      - id: ruff-format
```

### VS Code Integration

```json
{
  "python.linting.enabled": true,
  "python.linting.ruffEnabled": true,
  "python.formatting.provider": "none",
  "[python]": {
    "editor.defaultFormatter": "charliermarsh.ruff",
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "source.fixAll.ruff": true,
      "source.organizeImports.ruff": true
    }
  }
}
```

### Custom Rule Selection

```bash
# Select specific rule categories
ruff check --select E4,E7,E9,F .

# Complex selection with ignores
ruff check --select ALL --ignore ANN,D,ERA .

# Per-file configuration via command line
ruff check --per-file-ignores "__init__.py:F401,tests/*:S101" .
```

### Output Formats

```bash
# JSON output
ruff check --output-format json . > violations.json

# GitHub Actions format
ruff check --output-format github .

# Text format with file paths
ruff check --output-format text .

# Grouped by file
ruff check --output-format grouped .
```

### Performance Tips

1. **Use `.ruffcache`**: Ruff automatically caches results
1. **Exclude unnecessary directories**: Configure `exclude` properly
1. **Use `--select` instead of `--ignore`**: More explicit rule selection
1. **Run in parallel**: Ruff automatically parallelizes checking

### Migration from Other Tools

#### From Flake8

```toml
# Flake8 config migration
[tool.ruff.lint]
select = ["E", "F", "W", "C90"]
ignore = ["E203", "E266", "E501", "W503"]
max-line-length = 88
max-complexity = 18
```

#### From Black

```toml
[tool.ruff.format]
quote-style = "double"
indent-style = "space"
line-ending = "auto"
```

#### From isort

```toml
[tool.ruff.lint.isort]
force-single-line = true
force-sort-within-sections = true
known-first-party = ["myproject"]
```

## Troubleshooting

### Common Issues

1. **Rule conflicts**: Use `--diff` to see what changes would be made
1. **Performance issues**: Check `.ruffcache` directory, exclude large dirs
1. **Configuration not found**: Ensure `pyproject.toml` is in project root
1. **Import sorting issues**: Configure `[tool.ruff.lint.isort]` section

### Debug Commands

```bash
# Show current configuration
ruff check --show-settings .

# Show which files are being processed
ruff check --show-files .

# Get statistics about violations
ruff check --statistics .

# Show explanation for a rule
ruff rule F401
```

## Quick Reference Summary

### Most Useful Commands

```bash
ruff check --fix .           # Lint and fix
ruff format .                # Format code
ruff check --select ALL .    # Check everything
ruff check --statistics .    # Get violation stats
```

### Essential Configuration

```toml
[tool.ruff]
line-length = 88
target-version = "py38"

[tool.ruff.lint]
select = ["E", "F", "UP", "B", "I"]
ignore = ["E501"]
fixable = ["ALL"]
```

### Key Rule Categories

- **E/W**: Style and formatting (pycodestyle)
- **F**: Logic errors (Pyflakes)
- **UP**: Python version upgrades (pyupgrade)
- **B**: Bug-prone patterns (flake8-bugbear)
- **I**: Import sorting (isort)
- **S**: Security issues (flake8-bandit)
- **D**: Documentation (pydocstyle)