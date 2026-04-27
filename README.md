# Лабораторная работа №31-32. Entity Framework Core

## Информация о студенте
- ФИО: Щербаков Данил Николаевич
- Группа: ИСП-232
- Дата: 27.04.2026

## Описание работы
Создано Web API для управления задачами с использованием Entity Framework Core и SQLite.

## Полезные команды dotnet ef

| Команда | Описание |
|---------|----------|
| `dotnet ef migrations add <Name>` | Создать новую миграцию |
| `dotnet ef database update` | Применить миграции к БД |
| `dotnet ef migrations list` | Список миграций |
| `dotnet ef migrations remove` | Удалить последнюю миграцию |
| `dotnet ef database update <MigrationName>` | Откатить до указанной миграции |

## Структура проекта

TaskDb/
├── Models/
│ ├── TaskItem.cs
│ └── TaskDtos.cs
├── Data/
│ └── AppDbContext.cs
├── Controllers/
│ └── TasksController.cs
├── Migrations/
├── taskdb.db
└── Program.cs


## Список маршрутов

| Маршрут | Метод | Описание |
|---------|-------|----------|
| /api/tasks | GET | Получить все задачи |
| /api/tasks/{id} | GET | Получить задачу по ID |
| /api/tasks | POST | Создать задачу |
| /api/tasks/{id} | PUT | Обновить задачу |
| /api/tasks/{id}/complete | PATCH | Переключить статус |
| /api/tasks/{id} | DELETE | Удалить задачу |
| /api/tasks/search | GET | Поиск задач |
| /api/tasks/stats | GET | Статистика |
| /api/tasks/paged | GET | Пагинация |
| /api/tasks/overdue | GET | Просроченные задачи |
| /api/tasks/complete-all | PATCH | Завершить все задачи |
| /api/tasks/completed | DELETE | Удалить выполненные |

## Таблица миграций

| Миграция | Описание |
|----------|----------|
| InitialCreate | Создание таблицы Tasks |
| AddDueDateToTask | Добавление поля DueDate |

## LINQ vs SQL

| LINQ | SQL |
|------|-----|
| `.Where(t => t.IsCompleted == false)` | `WHERE is_completed = 0` |
| `.OrderBy(t => t.CreatedAt)` | `ORDER BY created_at ASC` |
| `.OrderByDescending(t => t.CreatedAt)` | `ORDER BY created_at DESC` |
| `.Take(10)` | `LIMIT 10` |
| `.Skip(20).Take(10)` | `OFFSET 20 LIMIT 10` |
| `.Count()` | `SELECT COUNT(*)` |
| `.GroupBy(t => t.Priority)` | `GROUP BY priority` |

## Главные выводы

1. **EF Core — переводчик между C# и SQL**: LINQ-запросы автоматически преобразуются в SQL.

2. **Миграции — контроль версий для БД**: Изменения структуры сохраняются в Git.

3. **Code First удобен для разработки**: Изменил класс → создал миграцию → БД обновлена.

4. **SaveChangesAsync() — ключевой момент**: До вызова все изменения живут только в памяти.

5. **async/await стандарт для работы с БД**: Не блокирует потоки сервера.

## Сравнение: память vs SQLite

| Концепция | Хранение в памяти | EF Core + SQLite |
|-----------|-------------------|------------------|
| Хранение данных | static List<T> в RAM | Файл .db на диске |
| После перезапуска | Данные пропадают | Данные сохраняются |
| Поиск по условию | LINQ to Objects | LINQ to Entities → SQL |
| Создание структуры | Не нужно | Миграции |
| Начальные данные | Хардкод в коде | HasData() в миграции |