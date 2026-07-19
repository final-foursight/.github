# Contributing

final-foursight is split across several repos, each owning one layer of the pipeline (see the [org profile](profile/README.md) for the full layout). This file is the default contributing guide for any final-foursight repo that doesn't define its own.

## Getting started

Each repo manages its own dependencies and Python version with [`uv`](https://github.com/astral-sh/uv):

```
uv sync
uv run pytest
```

## Before opening a PR

Every repo runs the same shared CI checks (defined in [`final-foursight/.github`](https://github.com/final-foursight/.github)):

- **Tests** — `uv run pytest`
- **Coverage** — tracked via Codecov
- **mypy** — `uv run mypy .`
- **Ruff** — `uv run ruff check .` and `uv run ruff format --check .`
- **pre-commit** — `uv run pre-commit run --all-files`

Run these locally before pushing; the same checks gate PRs in CI.

## Pull requests

- Keep PRs scoped to one repo/layer where possible.
- Fill out the PR template — note which repo(s) are affected.
- Reference any related issues.
