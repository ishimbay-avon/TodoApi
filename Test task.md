# Тестовое задание для Junior C# Backend Developer

## Задача
Разработать микросервис для управления задачами (Todo API) с использованием .NET Core, PostgreSQL и Docker.

## Технологии
- .NET Core 6+
- PostgreSQL
- Entity Framework Core (Code-First)
- Docker
- Опционально: Redis, RabbitMQ, gRPC

## Базовые требования

### Функционал API
- **CRUD операции для задач:**
  ```json
  {
    "id": 1,
    "title": "Изучить C#",
    "description": "Async/await",
    "status": "active", // или "completed"
    "created_at": "2024-06-10T12:00:00Z"
  }
  ```
- Поиск задач по названию (регистронезависимый)
- Фильтрация по статусу (active, completed)
- Пагинация для GET-методов
- Асинхронные методы контроллеров

### Инфраструктура
- Docker-контейнеризация приложения и БД
- Автоматическое применение миграций EF Core при запуске
- Docker Compose для запуска стека

## Дополнительные задания (выполнение повышает оценку)
### Вариант A: Redis (кеширование)
- Закешировать ответы GET-методов:
    - GET /api/tasks - кеш на 5 минут
    - Инвалидация кеша при изменении данных (POST/PUT/DELETE)

### Вариант B: RabbitMQ (асинхронная обработка)
- Отправка события в RabbitMQ при изменении статуса задачи:

```csharp
Publish("task.status.changed", new { TaskId = 1, NewStatus = "completed" });
```
### Вариант C: gRPC (интеграция)
- gRPC-сервис для статистики:

```proto
service TodoAnalytics {
  rpc GetStats (StatsRequest) returns (StatsResponse);
}
message StatsResponse {
  int32 active_tasks = 1;
  int32 completed_tasks = 2;
}
```
### Вариант D: Unit-тесты
- Покрытие тестами (xUnit/NUnit):
    - Контроллеры (минимум 70% coverage)
    - Сервисы (логику изменения статуса)

## Требуемая инфраструктура
###Docker Compose
Пример ```docker-compose.yml```:

```yaml
services:
  api:
    build: .
    ports: ["8080:80"]
    depends_on: 
      - postgres
    environment:
      ConnectionStrings__Default: "Host=postgres;Database=todo;Username=postgres;Password=password"

  postgres:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: "password"
      POSTGRES_DB: "todo"
```
## Что предоставить
### 1. GitHub-репозиторий с:
- Исходным кодом приложения
- ```Dockerfile```
- ```docker-compose.yml```
### 2. README.md с:
- Инструкцией по запуску
- Описанием реализованных дополнительных заданий
- Примеры запросов (cURL/Postman)
- Объяснением архитектурных решений