# claude-python-template

A Python project template designed for developing with [Claude Code](https://claude.ai/code) inside a containerized environment.

## Prerequisites

- Python 3.13+
- [uv](https://docs.astral.sh/uv/) package manager
- [Podman](https://podman.io/) (for container workflow)

## Quick Start

```bash
uv sync
uv run claude-python-template
```

## Development

### Commands

| Command | Description |
|---|---|
| `uv sync` | Install dependencies |
| `uv run claude-python-template` | Run the CLI |
| `uv build` | Build the package |
| `uv run ruff check .` | Lint |
| `uv run ruff format .` | Format |
| `uv run ty check` | Type check |
| `uv run pre-commit run --all-files` | Run all pre-commit hooks |

### Pre-commit Hooks

Pre-commit is configured with:

- **ruff** &mdash; linting (`--fix --exit-non-zero-on-fix`) and formatting
- **ty** &mdash; type checking
- Trailing whitespace removal, end-of-file fixer, YAML/TOML validation, LF line endings, large file and private key detection

## Container Workflow

The repo includes scripts for running Claude Code inside an isolated Podman container with the full toolchain pre-installed (uv, ruff, ty, Node.js, ripgrep, etc.).

### Build the container image

```bash
bin/build-container           # builds claude-uv:latest
bin/build-container my-image  # custom image name
```

The Python version is auto-detected from `pyproject.toml`.

### Run Claude Code in the container

```bash
bin/run-claude                # interactive session
bin/run-claude --help         # pass flags to claude
```

The script mounts the repo, forwards credentials, and persists per-repo state (`.claude/`, uv cache, virtualenvs) across sessions. Old virtualenvs are pruned after 7 days.

#### Environment variables

| Variable | Description |
|---|---|
| `CLAUDE_IMAGE` | Override the container image (default: `claude-uv:latest`) |
| `CLAUDE_SAFE_MODE=1` | Run **with** the permission system enabled (default skips permissions) |

## Project Structure

```
src/claude_python_template/   # Package source
bin/
  build-container             # Build the Podman image
  run-claude                  # Run Claude Code in container
plans/                        # Plan files (auto-synced from ~/.claude/plans/)
Containerfile.claude          # Container definition
pyproject.toml                # Project config, dependencies, tool settings
```
