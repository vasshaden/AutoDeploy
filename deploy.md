# Справка по GitHub Actions Deploy Workflow

## Описание
Автоматический workflow для сборки и публикации Docker-образа в GitHub Container Registry при каждом push в ветку `main`.

## Структура файла `.github/workflows/deploy.yml`

### Триггер
```yaml
on:
  push:
    branches: [ main ]
```
- Запускается только при push в ветку `main`

### Переменные окружения
```yaml
env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}
```
- `REGISTRY` - адрес GitHub Container Registry
- `IMAGE_NAME` - имя образа (автоматически берется из названия репозитория)

### Job: build-and-push

**Платформа:** `ubuntu-latest`

**Права доступа:**
- `contents: read` - чтение кода репозитория
- `packages: write` - публикация в GitHub Packages

### Шаги выполнения

1. **Checkout repository** (`actions/checkout@v4`)
   - Скачивает код репозитория

2. **Log in to Container Registry** (`docker/login-action@v3`)
   - Авторизация в ghcr.io
   - Использует `GITHUB_TOKEN` (автоматически предоставляется)

3. **Extract metadata** (`docker/metadata-action@v5`)
   - Генерирует теги для образа:
     - `latest` - всегда последняя версия
     - По имени ветки (например, `main`)
     - По SHA коммита (например, `sha-abc1234`)

4. **Build and push Docker image** (`docker/build-push-action@v5`)
   - Собирает образ из Dockerfile в корне проекта
   - Публикует образ с сгенерированными тегами

## Результат
После успешного выполнения образ доступен по адресу:
```
ghcr.io/USERNAME/REPOSITORY:latest
ghcr.io/USERNAME/REPOSITORY:main
ghcr.io/USERNAME/REPOSITORY:sha-COMMIT_HASH
```

## Использование
1. Запушить код в ветку `main`
2. Workflow запустится автоматически
3. Проверить статус в разделе "Actions" на GitHub
4. Использовать опубликованный образ в docker-compose или других проектах