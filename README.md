
# 📝 Laravel To-Do API

Простое RESTful API для управления задачами (CRUD: создание, чтение, обновление, удаление).  
Проект работает в Docker-контейнерах с Nginx, PHP и MySQL.

---

## 📦 Требования

- Docker
- Docker Compose

---

## 🚀 Установка и запуск

### 1. Склонируйте репозиторий

```bash
git clone <ваш-репозиторий>
cd test-todo
```

> Если проект уже у вас локально — просто перейдите в папку проекта.

### 2. Запустите контейнеры

```bash
docker-compose up -d
```

Это запустит:
- **nginx** (порт 8000 на хосте)
- **php**
- **mysql** (порт 3306)

### 3. Выполните миграции

```bash
docker-compose exec php php artisan migrate
```

> При первом запуске база данных будет пустой. Миграции создадут таблицу `tasks`.

---

## 🧪 Как проверялся проект

Все эндпоинты тестировались через **Postman**:

| Метод | URL | Описание |
|------|-----|--------|
| `GET`    | `http://localhost:8000/api/tasks`       | Получить список задач |
| `POST`   | `http://localhost:8000/api/tasks`       | Создать новую задачу |
| `GET`    | `http://localhost:8000/api/tasks/1`     | Получить задачу по ID |
| `PUT`    | `http://localhost:8000/api/tasks/1`     | Обновить задачу |
| `DELETE` | `http://localhost:8000/api/tasks/1`     | Удалить задачу |

**Пример тела POST-запроса:**
```json
{
  "title": "Купить молоко",
  "description": "Срочно, к утру",
  "status": "pending"
}
```

> 💡 Убедитесь, что в Postman выбран **правильный HTTP-метод** и **только URL** (без слова `POST` в адресной строке).

---

## ⚠️ Важно

- API доступно по адресу: `http://localhost:8000/api/...`
- Все маршруты находятся в `routes/api.php`
- Для работы Form Request (`StoreTaskRequest`, `UpdateTaskRequest`) убедитесь, что метод `authorize()` возвращает `true`:
  ```php
  public function authorize(): bool
  {
      return true;
  }
  ```

---

Готово! Теперь вы можете управлять задачами через API. 🎉
