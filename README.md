# Добро пожаловать в проект TodoApi!

TodoApi - микросервис для управления задачами.

## 🛠️ Технологии

| Категория      | Технологии                           |
|----------------|--------------------------------------|
| Бэкенд         | .NET 8.0                             |
| Базы данных    | PostgreSQL, Entity Framework Core    |
| Инструменты    | Postman, pgAdmin 4, Docker Desktop   |


## ⚙️ API роуты

- GET /api/tasks - получить список задач
- GET /api/tasks/{id} - получить задачу по Id 
- POST /api/tasks - создать задачу
- PUT /api/tasks/{id} - обновить задачу по Id
- DELETE /api/tasks/{id} - удалить задачу по Id

## 📦 Структура проекта

- TodoApi/ - исходный код:
  - Controllers/ — контроллеры API, включая TasksController.cs
  - Data/ — класс контекста базы данных (AppDbContext.cs)
  - Models/ — описание моделей данных (например, TodoTask.cs)
  - Migrations/ — EF Core миграции (создаются автоматически)
  - Properties/ — файлы свойств проекта (например, launchSettings.json)
  - TodoApi.csproj — файл проекта .NET Core
  - Program.cs — точка входа, настройка приложения и сервисов
  - Dockerfile — описание сборки Docker образа
  - docker-compose.yml — описание сервисов Docker Compose
  - appsettings.json — настройки приложения (строка подключения БД)
  - README.md — описание проекта, инструкции по запуску
  - .dockerignore — для исключения файлов из контекста Docker сборки

## 📥 Установка и запуск

1. Скачать и установить .NET 8 SDK с официального сайта https://dotnet.microsoft.com

2. Убедитесь, что установлен DockerDesktop с официального сайта https://www.docker.com/products/docker-desktop/

3. Запустите Docker Compose для поднятия базы данных и приложения:
```
docker-compose up -d --build
```

4. Проверьте, что контейнеры работают корректно:
```
docker-compose ps
```

Приложение будет доступно по адресу: http://localhost:8080

## ☕ Проверка маршрутов

1. GET /api/tasks

Получение списка задач с фильтрами. Ответ содержит список задач.

Пример запроса:
```
GET http://localhost:8080/api/tasks?search=читать&status=active&page=1&pageSize=5
```

Параметры:

search (string, необязательный) — поиск по названию задачи, регистронезависимый.

status (string, необязательный) — статус задачи (active или completed).

page (int, необязательный) — номер страницы, по умолчанию 1.

pageSize (int, необязательный) — количество задач на странице, по умолчанию 10.

2. GET /api/tasks/{id}

Получение задачи по ID. Ответ — объект задачи или 404, если задача не найдена.

Пример запроса:
```
GET http://localhost:8080/api/tasks/1
```

3. POST /api/tasks

Создание новой задачи. Ответ — созданная задача с полем id и createdAt.

Пример тела запроса:
```
json
{
  "title": "Изучить C#",
  "description": "Асинхронное программирование",
  "status": "active"
}
```

4. PUT /api/tasks/{id}

Обновление существующей задачи. Ответ — HTTP 204 No Content при успешном обновлении, 404 если не найдена.

Пример тела запроса:
```
json
{
  "title": "Изучить C# продвинуто",
  "description": "Async/await и потоковая обработка",
  "status": "completed"
}
```

5. DELETE /api/tasks/{id}

Удаление задачи по ID. Ответ — HTTP 204 No Content при успехе, 404 если не найдена.

Пример запроса:
```
DELETE http://localhost:8080/api/tasks/1
```
## 📝 Объяснение архитектурных решений

В проекте используются:
- атоматическое применение миграций — при старте приложения создается база данных.
- пагинация, фильтрация и поиск — позволяют эффективно обрабатывать большие объёмы данных.

Контроллер

В проекте реализован контроллер TasksController, который обрабатывает HTTP-запросы по маршруту api/tasks.
Контроллер взаимодействует с базой данных через AppDbContext с помощью Entity Framework Core.

Data-модель

Модель используется для создания таблицы в базе и для передачи данных через API.
Модель данных TodoTask описывает структуру задачи:
- Id — уникальный идентификатор задачи.
- Title — заголовок задачи.
- Description — описание задачи.
- Status — статус задачи, например, "active" или "completed".
- CreatedAt — дата и время создания задачи.

```json
  {
    "id": 1,
    "title": "Изучить C#",
    "description": "Async/await",
    "status": "active", // или "completed"
    "created_at": "2024-06-10T12:00:00Z"
  }
```