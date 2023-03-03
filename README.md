1. Описание


Проект предназначен для взаимодействия с API социальной сети YaMDb. YaMDb собирает отзывы пользователей на различные произведения.

API предоставляет возможность взаимодействовать с базой данных по следующим направлениям:

авторизироваться
создавать свои отзывы и управлять ими (корректировать\удалять)
просматривать и комментировать отзывы других пользователей
просматривать комментарии к своему и другим отзывам
просматривать произведения, категории и жанры произведений


2. Установка Docker (на платформе Ubuntu)


Проект поставляется в трёх контейнерах Docker (db, web, nginx). Для запуска необходимо установить Docker и Docker Compose.

Подробнее об установке на других платформах можно узнать на официальном сайте.

-Для начала необходимо скачать и выполнить официальный скрипт:

apt install curl
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh

-При необходимости удалить старые версии Docker:

apt remove docker docker-engine docker.io containerd runc

-Установить пакеты для работы через протокол https:

apt update
apt install \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg-agent \
  software-properties-common -y
  
-Добавить ключ GPG для подтверждения подлинности в процессе установки:

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

-Добавить репозиторий Docker в пакеты apt и обновить индекс пакетов:

add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" 
apt update

-Установить Docker(CE) и Docker Compose:

apt install docker-ce docker-compose -y

-Проверить что Docker работает можно командой:

systemctl status docker
Подробнее об установке можно узнать по ссылке.


3. Переменные окружения


Проект использует базу данных PostgreSQL.
Для подключения и выполненя запросов к базе данных необходимо создать и заполнить файл ".env" с переменными окружения в папке "./infra/".

-Шаблон для заполнения файла ".env":

DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
SECRET_KEY='Здесь указать секретный ключ'


4. Команды для запуска


Перед запуском необходимо склонировать проект:

HTTPS: git clone https://github.com/.../yamdb.git
SSH: git clone git@github.com:.../yamdb.git

-Cоздать и активировать виртуальное окружение:

python -m venv venv
Linux: source venv/bin/activate
Windows: source venv/Scripts/activate

-установить зависимости из файла requirements.txt:

python3 -m pip install --upgrade pip
pip install -r requirements.txt

-Из папки "./infra/" выполнить команду создания и запуска контейнеров:

docker-compose up --build

-После успешного запуска контейнеров выполнить миграции:

docker-compose exec web python manage.py migrate

-Создать суперюзера:

docker-compose exec web python manage.py createsuperuser

-Собрать статику:

docker-compose exec web python manage.py collectstatic --no-input 
Теперь доступность проекта можно проверить по адресу http://localhost/admin/


5. Заполнение базы данных


Скопировать файл с дампом базы данных из папки "./infra/" в контейнер:

docker cp fixtures.json infra_web_1:/app/fixtures.json

-Заполнить базу данных из файла с дампом:

docker-compose exec web python manage.py loaddata fixtures.json


6. Техническая информация


Стек технологий: Python 3, Django, Django Rest, Docker, PostgreSQL, nginx, gunicorn, simple JWT.

Веб-сервер: nginx (контейнер nginx)
Backend фреймворк: Django (контейнер web)
API фреймворк: Django REST (контейнер web)
База данных: PostgreSQL (контейнер db)

Веб-сервер nginx перенаправляет запросы клиентов к контейнеру web, либо к хранилищам (volume) статики и файлов.
Контейнер nginx взаимодействует с контейнером web через gunicorn.

7. Об авторе
Кошельник Виктория Викторовна

Python-разработчик (Backend)

Россия, г. Москва

E-mail: viktoriakoshelnik@yandex.ru

Telegram: @ViktoriaKosh
