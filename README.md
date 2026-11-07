# Kittygram — контейнеризация и автоматизация CI/CD

Проект по контейнеризации веб-приложения Kittygram и настройке автоматического тестирования и развёртывания на удалённый сервер.

Исходный код приложения предоставлен в рамках образовательного задания. Моя работа заключалась в подготовке Docker-инфраструктуры и настройке CI/CD с использованием GitHub Actions.

## Мой вклад

- Подготовил Dockerfile для backend-приложения на Django.
- Настроил Docker Compose для совместной работы backend, frontend, Nginx и PostgreSQL.
- Настроил Docker volumes для хранения статических файлов, пользовательских медиафайлов и данных PostgreSQL.
- Настроил подключение Django к PostgreSQL и передачу параметров через переменные окружения.
- Настроил Nginx для работы внутри Docker и проксирования запросов к backend.
- Настроил GitHub Actions для автоматического тестирования backend и frontend.
- Добавил проверку backend-кода на соответствие PEP 8.
- Настроил сборку Docker-образов и их публикацию в Docker Hub после успешного прохождения проверок.
- Настроил автоматическое обновление образов и перезапуск контейнеров на удалённом сервере.
- Добавил выполнение миграций и сборку статических файлов при деплое.
- Настроил Telegram-уведомления об успешном завершении развёртывания.

## Технологии

- Python 3.12
- Django 5.1
- PostgreSQL
- Docker
- Docker Compose
- GitHub Actions
- Docker Hub
- Nginx
- Linux
- SSH

## Архитектура контейнеров

Приложение разворачивается с использованием четырёх контейнеров:

| Контейнер | Назначение |
|---|---|
| `backend` | Django-приложение и REST API |
| `frontend` | Клиентская часть приложения |
| `gateway` | Nginx: обслуживание frontend, статики и media, проксирование запросов |
| `db` | PostgreSQL |

Для хранения данных используются Docker volumes:

- `static` — статические файлы приложения.
- `media` — загружаемые пользователями файлы.
- `pg_data` — данные PostgreSQL.

## CI/CD

Автоматизация реализована с помощью GitHub Actions.

При push в ветку `main` запускается workflow, который выполняет следующие этапы:

1. Проверяет backend-код на соответствие PEP 8.
2. Запускает тесты backend и frontend.
3. Собирает Docker-образы backend, frontend и gateway.
4. Публикует собранные образы в Docker Hub.
5. Подключается к удалённому серверу по SSH.
6. Загружает обновлённые образы и перезапускает контейнеры с помощью Docker Compose.
7. Выполняет миграции базы данных и сборку статических файлов.

Workflow запускается автоматически после push в `main`.

## Структура проекта

```text
kittygram-docker-cicd/
├── backend
│   ├── cats/
│   ├── Dockerfile
│   ├── kittygram_backend/
│   ├── manage.py
│   ├── README.md
│   └── requirements.txt
├── frontend
│   ├── Dockerfile
│   ├── package.json
│   ├── package-lock.json
│   ├── public/
│   ├── README.md
│   └── src/
├── nginx
│   ├── Dockerfile
│   └── nginx.conf
├── tests/
├── docker-compose.production.yml
├── docker-compose.yml
├── kittygram_workflow.yml
├── LICENSE
├── pytest.ini
├── README.md
```

## Результат

Настроена контейнеризация Kittygram и автоматизирован полный цикл доставки приложения: от проверки кода и запуска тестов до сборки Docker-образов, публикации в Docker Hub и развёртывания обновлённой версии на удалённом сервере.