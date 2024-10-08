# Taski

### О возможностях проекта
Taski — сервис для создания задач/заметок.

### О проекте
Проект можно развернуть на локальном сервере с использованием системы контейнеризации при помощи утилиты `Docker Compose`. Архитектура контейнеризации проекта поделена на отдельные зоны ответственности: `backend`, `frontend`, `gateway` и `db`.<br>
Статика для бэкенда и фронтенда, медиафайлы, а также база данных, расположены в отдельных томах (volume): `static`, `media` и `pg_data`, независимо от контейнеров.<br>
При разворачивании проекта на удалённом сервере, в случае изменения кода в главной ветке (main) репозитория, произойдёт автоматическое тестирование кода и автоматическое развертывание новой версии на удалённом сервере с помощью `GitHub Actions`. При изменинии кода в любой другой ветке репозитория, произойдёт только автоматическое тестирование кода<br>

### Как самостоятельно развернуть проект
Для развертывания проекта необходимо в корневой директории проекта создать файл `.env` и добавить в него необходимые данные для переменных, по аналогии с файлом `.env.example`, который присутствует в репозитории.<br>

Внимание! На локальном сервере должны быть установлены утилиты Docker и Docker Compose.<br>

Запускаем Docker Compose на сервере:
```shell
sudo docker compose up -d
```
После запуска выполняем миграции, собираем статические файлы бэкенда и копируем их в /backend_static/static/:
```shell
sudo docker compose exec backend python manage.py migrate
sudo docker compose exec backend python manage.py collectstatic
sudo docker compose exec backend cp -r /app/collected_static/. /backend_static/static/
```

Далее можно перейти на [главную страницу](http://127.0.0.1:8000/) и опробовать функционал

Для остановки контейнеров:
```shell
sudo docker compose down
```
