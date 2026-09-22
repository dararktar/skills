---
name: python-uv
description:
  Guide for using uv, the Python package and project manager. Use this when
  working with Python projects, scripts, packages, or tools.
---

# uv

uv - это менеджер пакетов и менеджер проекта, замена pip, pipenv, poetry, venv и т.д

## Когда использовать

**Всегда использовать uv**, особенно если в проекте есть:
- файл uv.lock
- в файле pyproject.toml в разделе dependency-groups[dev] имеется зависимость uv.
- отсутствуют файлы pyproject.toml и requirements.txt

Не используйте UV-развертку в проектах, управляемых другими инструментами:
- Poetry проекты (идентифицируются по файлу `poetry.lock`)
- Проекты PDM (идентифицируются по файлу `pdm.lock`)

## Рабочий процесс

### Скрипты

```bash
uv run script.py # Запуск скрипта
uv run --with requests script.py # Запуск с дополнительными пакетами
uv add --script script.py requests # Добавляем зависимости непосредственно в скрипт
```

### Проекты

**Использовать, когда:** Присутствует файл `pyproject.toml` или `uv.lock`

**Основные команды:**

```bash
uv init # Создать новый проект
uv add requests # Добавить зависимость
uv remove requests # Удалить зависимость
uv sync # Установка из файла блокировки
uv run <command> # Выполнение команд в среде
uv run python -c "" # Запуск Python в среде проекта
uv run -p 3.14 <command> # Запуск с указанной версией Python
```

### Инструменты

**Используйте, когда:** Запускаете инструменты командной строки (например, ruff, ty, pytest) без
установки.

**Основные команды:**

```bash
uvx <tool> <args> # Запуск инструмента без установки
uvx <tool>@<version> <args> # Запустить определенную версию инструмента
```

**Важно:**

- `uvx` запускает инструменты из PyPI по имени пакета. Это может быть небезопасно — запускайте только
  хорошо известные инструменты.
— Используйте команду `uv tool install` только по запросу пользователя.

## Распространенные шаблоны

### Не используйте pip в UV-проектах

```bash
# Плохой
pip install requests

# Хороший
uv add
```

### Не запускайте Python напрямую

```bash
# Плохой
python script.py

# Хороший
uv run script.py
```

```bash
# Плохой
python -c "..."

# Хороший
uv run python -c "..."
```

### Не управляйте окружениями вручную в UV-проектах

```bash
# Плохой
python -m venv .venv
source .venv/bin/activate

# Хороший
uv run <command>
```

## Документация

Для получения подробной информации ознакомьтесь с официальной документацией:

- https://docs.astral.sh/uv/llms.txt

В документации содержатся ссылки на конкретные страницы для каждого из этих рабочих процессов.
