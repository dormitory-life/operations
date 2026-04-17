# DormitoryLife stage

Готовый локальный стенд для запуска проекта одной командой без клонирования всех репозиториев.

## Требования

- Docker
- Docker Compose
- make

## Запуск

```bash
make up
```

## Остановка

```bash
make down
```

## Логи

```bash
make logs
```

## Адреса

Frontend: http://localhost:3000

Gateway API: http://localhost:8080

MinIO API: http://localhost:9000

MinIO Console: http://localhost:9001

RabbitMQ Management: http://localhost:15672

## Механизм работы

В docker-compose описан следующий порядок запуска сервисов:

1. db
2. storage
3. broker
4. cache
5. auth-service
6. core-service
7. gateway-service
8. frontend (nginx)

## Примечания

1. Тестирование отправки сообщения в поддержку не работает из-за сборки образа с конфигом без кредов smtp клиента.
2. Приложение будет доступно по адресу http://localhost:3000
