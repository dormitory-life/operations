# dormitory-life — локальный запуск проекта для разработки

Организация проекта: https://github.com/dormitory-life

## Структура проекта

Для локального запуска предполагается, что все репозитории лежат в одной общей директории.

## 1. Создайте папку для проекта

```bash
mkdir dormitory-life
cd dormitory-life
```

## 2. Склонируйте основные репозитории

```bash
git clone https://github.com/dormitory-life/auth.git
git clone https://github.com/dormitory-life/core.git
git clone https://github.com/dormitory-life/gateway.git
git clone https://github.com/dormitory-life/frontend.git
```

## 3. Склонируйте репозиторий `operations`

На данный момент локальное развертывание через готовые образы из Docker Hub не реализовано, поэтому необходимо дополнительно подтянуть локальные файлы конфигурации из репозитория `operations`.

```bash
git clone https://github.com/dormitory-life/operations.git
cp -r operations/local/* .
```

После этого корневая директория будет содержать все необходимые репозитории и файлы для локального запуска.

Структура коревой директории должна быть такой:

- auth
- core
- frontend
- gateway
- operations (если склонировали, а не скопировали файлы)
- docker-compose-infra-yaml
- docker-compose.yaml
- Makefile
- этот ридми

## 4. Требования для запуска

В зависимости от выбранного способа запуска понадобятся:

### Вариант 1. Запуск сервисов через бинарники

Потребуется:

- Go
- Docker
- Docker Compose
- Node.js
- npm
- make

### Вариант 2. Запуск сервисов в контейнерах

Потребуется:

- Docker
- Docker Compose
- Node.js
- npm
- make

> `Node.js` и `npm` нужны для запуска фронтенда, так как он стартует командой `npm run dev`.

## 5. Запуск проекта

### Вариант 1. Запуск сервисов через бинарники и инфраструктуры в Docker

Используйте этот способ, если у вас установлен Go.

Команда:

```bash
make project-up
```

Что делает эта команда:

- собирает и поднимает контейнеры с:
  - PostgreSQL
  - S3
  - RabbitMQ
- запускает сервисы `Auth`, `Core`, `Gateway` локально
- запускает фронтенд

---

### Вариант 2. Запуск всех бэкенд-сервисов в контейнерах

Используйте этот способ, если Go не установлен.

Команда:

```bash
make project-container-up
```

Что делает эта команда:

- создает docker-сети
- собирает и поднимает контейнеры с:
  - PostgreSQL
  - S3
  - RabbitMQ
- собирает и поднимает контейнеры с сервисами:
  - `Auth`
  - `Core`
  - `Gateway`
- запускает фронтенд

## 6. Остановка проекта

Если проект был запущен через бинарники:

```bash
make project-down
```

Если проект был запущен в контейнерах:

```bash
make project-container-down
```

## 7. Примечания

- для того, чтобы сообщение на почту поддержки отправлялось, требуется прописать в конфиге креды для smtp mail.ru
- кнопки для регистрации нет ввиду специфики проекта
- для теста есть тестовые учетные записи:
  1. Админ:
  - Email: admin9@mail.ru / admin1@mail.ru (для общежитий 9 и 1 соответственно)
  - Password: 555TGB5tgb
  2. Студент:
  - Email: test9@mail.ru / test1@mail.ru (для общежитий 9 и 1 соответственно)
  - Password: 555TGB5tgb
