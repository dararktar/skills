---
name: python-project
description: |
  Sets up professional Python library projects with modern tooling (pyproject.toml, uv, ruff, pytest, pre-commit, GitHub Actions). Use when creating new Python libraries, modernizing existing projects to pyproject.toml, configuring linting/testing/CI, or setting up Makefiles and pre-commit hooks.
---

# project

## Быстрый старт

Новый проект создается со следующей структурой

```
src
  app/
    __init__.py
  tests/
  py.typed
  pyproject.toml
```

## Минимальный pyproject.toml

```toml
[project]
name = "my-project"
version = "0.1.0"
description = "What it does"
readme = "README.md"
requires-python = ">=3.10"
license = {text = "MIT"}
dependencies = []

[build-system]
requires = ["uv_build>=0.12.5,<0.13.0"]
build-backend = "uv_build"

[tool.uv.build-backend]
module-name = ["app"]

[dependency-groups]
dev = ["pytest>=7.0", "ruff>=0.1", "basedpyright>=1.0"]
```

