# Инструкция к Dockerfile

## Описание
Dockerfile для создания контейнера с Flask приложением на Python 3.11.

## Структура Dockerfile

### Базовый образ
```dockerfile
FROM python:3.11-slim
```
Использует официальный Python 3.11 образ в slim версии для минимального размера.

### Рабочая директория
```dockerfile
WORKDIR /app
```
Устанавливает `/app` как рабочую директорию внутри контейнера.

### Установка зависимостей
```dockerfile
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
```
Копирует файл зависимостей и устанавливает пакеты без кеширования.

### Копирование приложения
```dockerfile
COPY app.py .
```
Копирует основной файл приложения в контейнер.

### Безопасность
```dockerfile
RUN useradd --create-home --shell /bin/bash appuser && \
    chown -R appuser:appuser /app
USER appuser
```
Создает непривилегированного пользователя и переключается на него.

### Конфигурация
```dockerfile
EXPOSE 5000
ENV FLASK_APP=app.py
ENV FLASK_ENV=production
```
- Открывает порт 5000
- Устанавливает переменные окружения Flask

### Запуск
```dockerfile
CMD ["python", "app.py"]
```
Команда запуска приложения.

## Сборка образа
```bash
docker build -t autodeploy .
```

## Запуск контейнера
```bash
docker run -p 5000:5000 autodeploy
```


или

## Сборка образа
```bash
docker build -t flask-test-app:latest .
для сборки docker-compose up -d в дальнейшем
```
