# Проект: запуск docker-compose
## Авторы:

[anthony-bogi](https://github.com/anthony-bogi)

## Описание:
### Проект YaMDb API

Проект YaMDb собирает отзывы пользователей на различные произведения.


## Технологии

Python 3.7

Django 3.2


## Установка и запуск проекта:

1. Проверить наличие Docker

   Прежде чем приступать к работе, убедиться что Docker установлен, для этого ввести команду:

   ```bash
   docker -v
   ```

   В случае отсутствия, скачать [Docker Desktop](https://www.docker.com/products/docker-desktop) для Mac или Windows. [Docker Compose](https://docs.docker.com/compose) будет установлен автоматически.

   В Linux проверить, что установлена последняя версия [Compose](https://docs.docker.com/compose/install/).

   Также можно воспользоваться официальной [инструкцией](https://docs.docker.com/engine/install/).

2. Клонировать репозиторий на локальный компьютер

   ```bash
   git clone https://git@github.com:anthony-bogi/infra_sp2.git 
   ```

3. В корневой директории создать файл `.env`, согласно примеру:

   ```bash
   DB_ENGINE=django.db.backends.postgresql
   DB_NAME=postgres
   POSTGRES_USER=postgres
   POSTGRES_PASSWORD=postgres
   DB_HOST=db
   DB_PORT=5432
   ```

4. Запустить `docker-compose`

   Выполнить из корневой директории команду:

   ```bash
   docker-compose up -d
   ```

5. Заполнить БД

   Создать и выполнить миграции:

   ```bash
   docker-compose exec web python manage.py makemigrations --noinput
   docker-compose exec web python manage.py migrate --noinput
   ```

6. Подгрузить статику

   ```bash
   docker-compose exec web python manage.py collectstatic --no-input
   ```

7. Заполнить БД тестовыми данными

   Для заполнения базы использовать файл `fixtures.json`, в директории `infra_sp2`. Выполните команду:

   ```bash
   docker-compose exec web python manage.py loaddata fixtures.json
   ```

8. Создать суперпользователя

   ```bash
   docker-compose exec web python manage.py createsuperuser
   ```

9. Остановить работу всех контейнеров

   ```bash
   docker-compose down
   ```

10. Пересобрать и запустить контейнеры

    ```bash
    docker-compose up -d --build
    ```

11. Мониторинг запущенных контейнеров

    ```bash
    docker stats
    ```

12. Остановить и удалить контейнеры, тома и образы

    ```bash
    docker-compose down -v
    ```



## Примеры запросов

Регистрация нового пользователя:
```
http://localhost/api/v1/auth/signup/
```
```
{
  "email": "string",
  "username": "string"
}
```

Получение JWT-токена:
```
http://localhost/api/v1/auth/token/
```
```
{
  "username": "string",
  "confirmation_code": "string"
}
```

> Токен необходимо передавать в заголовке каждого запроса, в поле Authorization. Перед самим токеном должно стоять ключевое слово Bearer и пробел.

* Основные эндпоинты API

Get-запросы

Получение списка всех категорий:
```
http://localhost/api/v1/categories/
```

Получение списка всех жанров:
```
http://localhost/api/v1/genres/
```

Получение списка всех произведений:
```
http://localhost/api/v1/titles/
```

Получение списка всех отзывов:
```
http://localhost/api/v1/titles/{title_id}/reviews/
```
> "title_id": integer - ID произведения

Получение списка всех комментариев к отзыву:
```
http://localhost/api/v1/titles/{title_id}/reviews/{review_id}/comments/
```
> "title_id": integer - ID произведения

> "review_id": integer - ID отзыва

Пример POST запроса добавления новой категории на (http://localhost/api/v1/categories/):
```
{
  "name": "string",
  "slug": "string"
}
```

## Требования:
```
asgiref==3.6.0
atomicwrites==1.4.1
attrs==22.2.0
certifi==2022.12.7
charset-normalizer==2.0.12
colorama==0.4.6
Django==3.2
django-filter==22.1
djangorestframework==3.12.4
djangorestframework-simplejwt==5.2.2
flake8==5.0.4
gunicorn==20.0.4
idna==3.4
importlib-metadata==4.2.0
iniconfig==2.0.0
isort==5.11.5
mccabe==0.7.0
packaging==23.0
pluggy==0.13.1
psycopg2-binary==2.8.6
py==1.11.0
PyJWT==2.1.0
pytest==6.2.4
pytest-django==4.4.0
pytest-pythonpath==0.7.3
python-dotenv==0.21.1
pytz==2022.7.1
requests==2.26.0
sqlparse==0.4.3
toml==0.10.2
typing_extensions==4.5.0
urllib3==1.26.14
zipp==3.13.0
```
