# Работа с Redis

Для работы с Redis были выполнены следующие шаги:
1. Запущен контейнер Redis с помощью команды: `docker run -d --name redis -p 6379:6379 redis:7`.
2. Для связи контейнеров n8n и Redis была создана общая сеть Docker:
   - `docker network create lab-net`
   - `docker network connect lab-net n8n`
   - `docker network connect lab-net redis`
3. В n8n создан Credential типа Redis. Host: `redis` (или IP-адрес контейнера), Port: `6379`.

## Создание и чтение ключа (flow-09-redis.json)
Процесс состоит из нод: Manual Trigger -> Redis Set (создает ключ `nosko` со значением строки) -> Redis Get (читает значение по ключу).
*Скриншот чтения значения прикреплен.*

## Удаление и проверка (flow-10-redis.json)
Процесс состоит из нод: Manual Trigger -> Redis Delete (удаляет ключ `nosko`) -> Redis Exists (проверяет наличие ключа).
В результате выполнения нода Redis Exists вернула `isExists: false`, что подтверждает успешное удаление.
*Скриншот проверки отсутствия прикреплен.*