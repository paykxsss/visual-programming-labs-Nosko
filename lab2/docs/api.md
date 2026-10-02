# API документация — Лабораторная работа №2 (Node-RED)

Базовый URL: `http://localhost:1880`

Все эндпоинты реализованы в flow `flow-08-endpoints.json`.

---

## 1. GET /api/text

Возвращает простой текст в кодировке UTF-8.

### Запрос

```
GET /api/text
```

### Параметры

Нет.

### Ответ 200 OK

**Content-Type:** `text/plain; charset=utf-8`

**Тело ответа:**

```
Привет! Это простой текст от Node-RED.
```

### Пример

Открыть в браузере:

```
http://localhost:1880/api/text
```

Или через curl:

```bash
curl http://localhost:1880/api/text
```

---

## 2. GET /api/info

Возвращает JSON-объект с двумя полями.

### Запрос

```
GET /api/info
```

### Параметры

Нет.

### Ответ 200 OK

**Content-Type:** `application/json; charset=utf-8`

**Тело ответа:**

```json
{
  "service": "Node-RED lab2",
  "version": "1.0.0"
}
```

### Пример

Открыть в браузере:

```
http://localhost:1880/api/info
```

Или через curl:

```bash
curl http://localhost:1880/api/info
```

---

## 3. GET /api/items/:id

Возвращает список товаров. Использует path-параметр `id` и опциональный query-параметр `limit`.

### Запрос

```
GET /api/items/:id?limit=N
```

### Параметры

| Имя     | Тип    | Расположение | Обязательный | Описание                                      |
|---------|--------|--------------|--------------|-----------------------------------------------|
| `id`    | число  | path         | да           | Целое положительное число                     |
| `limit` | число  | query        | нет          | Количество товаров, от 1 до 50 (по умолчанию 10) |

### Поведение

| Условие                              | Код ответа |
|--------------------------------------|------------|
| `id` не число или `id <= 0`          | 400        |
| `limit` задан, но не число или вне диапазона 1–50 | 400 |
| `id > 100` (товара нет)              | 404        |
| Иначе                                | 200        |

### Успешный запрос

```
GET /api/items/5?limit=3
```

**Ответ 200 OK:**

```json
{
  "requestedId": 5,
  "limit": 3,
  "count": 3,
  "items": [
    { "id": 500, "name": "Товар #5-0", "price": 123.45 },
    { "id": 501, "name": "Товар #5-1", "price": 678.90 },
    { "id": 502, "name": "Товар #5-2", "price": 11.22 }
  ]
}
```

Пример curl:

```bash
curl "http://localhost:1880/api/items/5?limit=3"
```

### Ошибка 400 — невалидный id

```
GET /api/items/abc
```

**Ответ 400 Bad Request:**

```json
{
  "error": "Invalid id",
  "message": "id должен быть положительным числом",
  "received": "abc"
}
```

Пример curl:

```bash
curl -i "http://localhost:1880/api/items/abc"
```

### Ошибка 400 — невалидный limit

```
GET /api/items/5?limit=999
```

**Ответ 400 Bad Request:**

```json
{
  "error": "Invalid limit",
  "message": "limit должен быть числом от 1 до 50",
  "received": "999"
}
```

Пример curl:

```bash
curl -i "http://localhost:1880/api/items/5?limit=999"
```

### Ошибка 404 — товар не найден

```
GET /api/items/999
```

**Ответ 404 Not Found:**

```json
{
  "error": "Not found",
  "message": "Товар с id=999 не найден",
  "maxId": 100
}
```

Пример curl:

```bash
curl -i "http://localhost:1880/api/items/999"
```

---

## Сводная таблица эндпоинтов

| Метод | URL                  | Коды ответов | Описание                          |
|-------|----------------------|--------------|-----------------------------------|
| GET   | `/api/text`          | 200          | Простой текст                     |
| GET   | `/api/info`          | 200          | JSON с двумя полями               |
| GET   | `/api/items/:id`     | 200, 400, 404| Список товаров с параметрами      |

---

## Как запустить

1. Убедиться, что Node-RED запущен: `http://localhost:1880`.
2. Импортировать `flow-08-endpoints.json` (Menu → Import).
3. Нажать **Deploy**.
4. Проверить эндпоинты через браузер или curl.