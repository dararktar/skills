---
name: python-quality
description:
  Improves Python library code quality through ruff linting, pyright/basedpyright type checking, Pythonic idioms, and refactoring. Use when reviewing code for quality issues, adding type hints, configuring static analysis tools, or refactoring Python library code.
---

# python quality

## Краткий справочник

```bash
# Запуск ruff
ruff check src && ruff format src
```

```bash
# Запуск pyright
pyright src
# Запуск basedpyright
basedpyright src
```

## Конфигурация ruff

Минимальная конфигурация ruff в pyproject.toml

```toml
[tool.ruff]
line-length = 88
target-version = "py312"

[tool.ruff.lint]
select = ["E", "W", "F", "I", "B", "C4", "UP"]
```

## Конфигурация pyright/basedpyright

```toml
[tool.pyright]
pythonVersion = "3.12"
typeCheckingMode = "strict"
reportImportCycles = false
reportPrivateUsage = false
reportCallInDefaultInitializer = true
reportImplicitStringConcatenation = true
```

## Организация модулей

```
src/my_library/
├── __init__.py      # Public API exports
├── _internal.py     # Private (underscore prefix)
├── exceptions.py    # Custom exceptions
├── types.py         # Type definitions
└── py.typed         # Type hint marker
```