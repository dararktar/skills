---
name: fastapi
description: |
  FastAPI is a modern, high-performance Python web framework for building APIs.
  It leverages Python type hints and Pydantic for automatic validation, serialization,
  and OpenAPI documentation generation with async/await support out of the box.
---

# fastapi

FastAPI — это веб-фреймворк на Python, построенный на основе Starlette (ASGI) и Pydantic. Он обеспечивает автоматическую проверку запросов, сериализацию и интерактивную документацию API. /docs(Swagger UI) и /redoc. 

## Установка

```bash
pip install fastapi[standart]
```

## Структура проекта

```
app/
  core/
    __init__.py
    config.py
    database.py
    dependencies.py
    exceptions.py
    logging.py
    middlewares.py
    storage.py
    utils.py
  <module>/
    models.py
    routers.py
    repositories.py
    schemas.py
    services.py
    tasks.py
  tests/
  __init__.py
  main.py
```

## Настройка проекта

### main.py

Инициализация fastapi приложения, с указанием lifespan. Здесь так же заполняются поля с названием, версией, описанием приложения.

### config.py

Описание класса Settings(BaseSettings) c возможностью загрузки настроек из переменных окружения и метод get_settings.

## Зависимости

Все зависимости описываются в core/dependencies.py.

## Бизнес логика

Бизнес логика описывается исключительно в services.py. Все инфраструктурные зависимости добавляются через параметры метода. Входные данные передаются в виде pydantic схемы из schemas.py. Результат: либо модель базы данных, либо датакласс, описанный в этом же файле, либо одиночное значение. Бизнес логика сторонних модулей вызывается ТОЛЬКО из services.py нужного модуля.

```
def get_user(data: GetUser, user_repo: UserRepository):
  user = user_repo.get_by_id(data.id)

  if not user:
    raise NotFoundError("Пользователь не найден")

  return user

## Роутеры

Схемы роутеров описываются в schema.py.

```python
from pydantic import BaseModel, Field

class ProductFilter(BaseModel):
    limit: int = Field(
        default=100, 
        gt=0, 
        le=100,
        title="Limit",
        description="Максимальное количество товаров в одной выборке",
        examples=[100],
    )
    offset: int = Field(
        default=0,
        ge=0,
        title="Offset",
        description="Смещение от начала списка товаров",
        examples=[0],
    )

    model_config = ConfigDict(
        title="Фильтр товаров",
        json_schema_extra={
            "description": "Параметры пагинации для получения списка товаров."
        },
    )
```

Роутеры описываются в routers.py c использованием собственного пространства имен. Для каждого из роутеров требуется полное описание openapi в декораторе. В роутере не должно быть ничего кроме вызова метода из services.py. Зависимости прописываются исключительно через Annotated.

```python
from fastapi import APIRouter, HTTPException, status, Query, Path
from .schemas import ProductFilter

@router.get(
    "/", 
    summary="Получить список товаров",
    description="Возвращает список о товаров с пагинацией`."
    response_model=list[ItemResponse]
    status_code=status.HTTP_200_OK,
    tags=["Items"],
)
async def list_items(filter: Annotated[ProductFilter, Query()], db_session: Depends(get_db_session)):
    return await get_items(db_session=db_session, offset=filter.offset, limit=filter.limit)
```