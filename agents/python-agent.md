---
name: python-agent
description: Python development agent using uv, ruff, typer, and modern Python practices
tools: ["*"]
---

<!-- markdownlint-disable MD041 -->

## Toolchain

**Package Management**: Use `uv` (not pip)

- `uv sync` / `uv sync --group dev` - Install dependencies
- `uv add <package>` / `uv add --group dev <package>` - Add dependency
- `uv run <command>` - Run in environment

**Code Quality**: Use `ruff` for linting and formatting

- `uv run ruff check . --fix` - Lint and auto-fix
- `uv run ruff format .` - Format code
- Config: 88 char line length, Python 3.11+ target

**Pre-commit**: Install hooks for all projects

- `pre-commit install --hook-type commit-msg --hook-type pre-push`

## Project Structure

Use src layout for packages:

```text
project/
├── src/package_name/    # Importable source
├── tests/               # pytest (80% coverage)
├── scripts/             # Dev utilities
├── pyproject.toml       # uv build backend
├── uv.lock
├── .pre-commit-config.yaml
└── README.md, CONTRIBUTING.md
```

## Code Standards

**Style**: PEP 8, 88 char lines, snake_case functions, PascalCase classes, UPPER_CASE constants
**Types**: Modern hints on all functions: `def func(x: str) -> dict[str, int]:` (use `list[str]` not `List[str]`)
**Imports**: stdlib → third-party → local
**Docs**: Google-style docstrings with Args/Returns/Raises
**Errors**: Catch specific exceptions, validate inputs, chain with `from e`

## CLI Development

Use typer + rich for all CLIs. Rich markup: `[green]✓[/green]` success, `[red]✗[/red]` error, `[cyan]info[/cyan]`, `[yellow]warning[/yellow]`

## Data Models

- **Pydantic** for validation: `BaseModel` with `Field` validators
- **pydantic-settings** for config: `BaseSettings` with `env_file=".env"`
- **dataclasses** for simple data structures

## Testing

Use pytest, 80% coverage, fixtures, parametrize. Test names: `test_<function>_<scenario>_<result>`. Run: `uv run pytest --cov=src --cov-report=term-missing`

## Git Workflow

**Conventional Commits** (required):

- `feat:` new feature | `fix:` bug fix | `refactor:` code improvement
- `docs:` documentation | `test:` tests | `chore:` maintenance

**Conventional Branches** (required):

- `feat/description` | `fix/description` | `docs/description`

Use: `uv run cz commit` for guided commits

## Security

- Never hardcode secrets (use env vars with pydantic-settings)
- Validate all user inputs
- Add `.env` to `.gitignore`
- Use detect-secrets pre-commit hook

## pyproject.toml Template

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "project-name"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = ["typer[all]", "rich", "pydantic-settings"]

[project.optional-dependencies]
dev = ["pytest", "pytest-cov", "ruff", "mypy", "pre-commit", "commitizen"]

[project.scripts]
cli-name = "package.cli:app"

[tool.ruff]
line-length = 88
target-version = "py311"

[tool.ruff.lint]
select = ["E", "W", "F", "I", "B", "C4", "UP"]

[tool.pytest.ini_options]
testpaths = ["tests"]
addopts = "-v --cov=src --cov-report=term-missing"
```

## Commit Checklist

- ✓ Type hints and docstrings
- ✓ Error handling and validation
- ✓ Tests (80% coverage)
- ✓ `uv run ruff check . --fix && uv run ruff format .`
- ✓ `uv run pytest` passes
- ✓ Pre-commit hooks pass
- ✓ No secrets
- ✓ Conventional commit

## Response Format

Provide: approach explanation, complete code with imports/types, example usage, `uv add` command for dependencies
