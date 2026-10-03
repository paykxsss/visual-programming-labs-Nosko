# Отчёт по лабораторной работе №2. Node-RED

## 1. Краткое описание выполненного

В рамках лабораторной работы №2 освоен Node-RED как low-code инструмент визуального программирования. Работа выполнена в Docker-контейнере с проброшенным volume на хост-машину.

Выполнены все пункты Части 2:

| № | Пункт | Файл flow |
|---|-------|-----------|
| 2.1 | Inject → Debug | `flow-01-inject-debug.json` |
| 2.2 | Function node | `flow-02-function.json` |
| 2.3 | Switch node | `flow-03-switch.json` |
| 2.4 | Change / Set node | `flow-04-change.json` |
| 2.5 | Template node (Mustache) | `flow-05-template.json` |
| 2.6 | HTTP Request node | `flow-06-http-request.json` |
| 2.7 | MQTT с публичным брокером | `flow-07-mqtt.json` |
| 2.8 | GET-эндпоинты | `flow-08-endpoints.json` |
| 2.9 | Dashboard (gauge + chart) | `flow-09-dashboard.json` |
| 2.10 | Telegram-бот | `flow-10-telegram.json` |
| 2.11 | Чтение и запись файла | `flow-11-files.json` |
| 2.12 | Работа с контекстом | `flow-12-context.json` |

Из Части 3 выбрана и выполнена **Ачивка 9** — API key + rate limit.

---

## 2. Какие AI-промпты использовались

Несколько ключевых примеров:

- **Для Function node (2.2):** «Сгенерируй код для Node-RED Function node, который использует let/const, if/else, цикл for, массив и объект, и возвращает объект с полем payload».
- **Для Template node (2.5):** «Сгенерируй Mustache-шаблон для Node-RED, который формирует чек заказа пиццы из объекта с полями customer, pizza, size, extras (массив), paid (булево), используя секции и инвертированные секции».
- **Для HTTP Request (2.6):** «Подскажи публичный API без регистрации, который возвращает JSON и стабильно работает из Docker-контейнера».
- **Для MQTT (2.7):** «Как настроить MQTT в Node-RED через публичный брокер broker.hivemq.com, чтобы сообщение уходило и возвращалось обратно».
- **Для ачивки 9:** «Сгенерируй Function node для Node-RED, которая проверяет заголовок X-API-Key и ограничивает частоту запросов до 3 в минуту на ключ с использованием flow context, возвращая 401 и 429».
- **Для отчёта:** «Помоги оформить report.md по лабораторной работе Node-RED со структурой: краткое описание, промпты, освоенные ноды, версии, выводы».

AI использовался как вспомогательный инструмент для генерации шаблонного кода, поиска API и оформления документации. Все flow проверены и задеплоены вручную, результат защищается студентом.

---

## 3. Какие ноды освоены

### Базовые

- `inject` — генерация сообщений (вручную, по интервалу, при деплое).
- `debug` — вывод сообщений в sidebar (payload, complete msg object).
- `function` — JS-код, работа с `msg`, `flow`, `global`.
- `switch` — ветвление по значению.
- `change` — установка и замена полей `msg`.
- `template` — Mustache-шаблоны (plain text, JSON).
- `http in` / `http response` — REST-эндпоинты.
- `http request` — исходящие HTTP-запросы к внешним API.
- `mqtt in` / `mqtt out` — публикация и подписка на MQTT-топики.
- `file` (file in / file out) — чтение и запись файлов.
- `comment` — комментарии на canvas.

### Dashboard

- `ui_tab` — вкладка на dashboard.
- `ui_group` — группа виджетов внутри вкладки.
- `ui_gauge` — стрелочный индикатор.
- `ui_chart` — график истории значений.

### Telegram

- `telegram bot` — конфигурация бота (токен от BotFather).
- `telegram receiver` — приём сообщений.
- `telegram command` — регистрация команд в BotFather.
- `telegram sender` — отправка сообщений.

### Контекст

- `flow.get` / `flow.set` — переменные в пределах flow.
- `global.get` / `global.set` — глобальные переменные.

---

## 4. Способ установки, версии Node-RED и Node.js

### Способ установки

Node-RED запущен в **Docker Desktop** на Windows / macOS.

Контейнер запускается командой:

```bash
docker run -d \
  --name node-red \
  -p 1880:1880 \
  -v node-red-data:/data \
  --dns 8.8.8.8 \
  --dns 1.1.1.1 \
  nodered/node-red
```

Volume `/data` проброшен на хост-машину, чтобы flows и settings сохранялись между перезапусками.

### Версии

Проверяются в логах контейнера при старте:

```bash
docker logs <CONTAINER_ID> | head -20
```

Пример вывода:

```
Welcome to Node-RED
===================
Node-RED version: v4.x.x
Node.js version: v20.x.x
```

Точные версии:

- **Node-RED:** `vX.Y.Z` (указать свою)
- **Node.js:** `vX.Y.Z` (указать свою)
- **Способ установки:** Docker (образ `nodered/node-red`)

Скриншот логов со версиями — в `screenshots/00-versions.png`.

---

## 5. Скриншоты всех flow

Все скриншоты лежат в `lab2/screenshots/`:

| № | Файл | Что показано |
|---|------|--------------|
| 00 | `00-versions.png` | Версии Node-RED и Node.js в логах Docker |
| 01 | `01-inject-debug.png` | Flow inject → debug |
| 02 | `02-function.png` | Flow inject → function → debug |
| 03 | `03-switch.png` | Flow с switch на два выхода |
| 04 | `04-change.png` | Flow inject → change → debug |
| 05 | `05-template.png` | Flow с Mustache-шаблоном |
| 06 | `06-http-request.png` | Flow с HTTP Request к внешнему API |
| 07 | `07-mqtt.png` | MQTT publish + subscribe |
| 08 | `08-endpoints-*.png` | Три GET-эндпоинта + ошибки |
| 09 | `09-dashboard.png`, `09-dashboard-flow.png` | Dashboard с gauge и chart |
| 10 | `10-telegram-chat.png`, `10-telegram-flow.png` | Чат с ботом + flow |
| 11 | `11-files-flow.png`, `11-files-debug.png` | Запись и чтение файла |
| 12 | `12-context-flow.png`, `12-context-debug.png` | Счётчик в flow context |
| 13 | `13-apikey-*.png` | API key: 401, 200, 429 + flow |

---

## 6. Выводы

В ходе лабораторной работы:

1. **Освоен Node-RED** как визуальный low-code инструмент: сборка flow из нод, деплой, отладка через sidebar.
2. **Изучены базовые ноды** — inject, debug, function, switch, change, template. Понял, что function даёт максимальную гибкость, а остальные ноды удобны для типовых операций.
3. **Разобрана работа с внешними сервисами** — HTTP Request к публичным API, MQTT через публичный брокер HiveMQ. MQTT показал, как устроен pub/sub: отправитель и получатель не знают друг о друге, общаются через брокер по топику.
4. **Собраны REST-эндпоинты** — три GET-эндпоинта с разными уровнями сложности: простой текст, JSON, параметры path/query с валидацией и статусами 200/400/404.
5. **Построен dashboard** с gauge и графиком — наглядно видно, как имитация датчика обновляет виджеты в реальном времени.
6. **Создан Telegram-бот** с командами `/start`, `/time` и echo-ответом. Понял, как работать с BotFather и нодами telegrambot.
7. **Разобрана работа с файлами** — запись и чтение через volume `/data`, проверено сохранение между перезапусками контейнера.
8. **Изучен контекст** — flow и global. Понял, что для сохранения между перезапусками нужен `contextStorage: localfilesystem`.
9. **Выполнена ачивка 9** — API key + rate limit. Реализована проверка заголовка `X-API-Key` и ограничение 3 запроса в минуту на ключ с возвратом 401 и 429. Это показало, как строить защищённые API без внешних зависимостей, только на function node и flow context.

**Главный вывод:** Node-RED позволяет быстро собирать прототипы интеграций — от простого inject→debug до REST API с авторизацией и Telegram-бота — без написания полноценного бэкенда. Для продакшена он тоже пригоден, но требует аккуратной работы с контекстом, версиями нод и безопасностью (токены, ключи).

---

## 7. Что можно улучшить

- Вынести токен Telegram-бота и API-ключи в environment variables, чтобы не хранить в JSON.
- Настроить `contextStorage: localfilesystem` для сохранения контекста между перезапусками.
- Добавить обработку ошибок через Catch node во все flow с внешними запросами.
- Для REST API — вынести логику в subflow, чтобы переиспользовать проверку ключа и rate limit на нескольких эндпоинтах.