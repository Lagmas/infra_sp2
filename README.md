# Проект YaMDb
## Описание
Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles). Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Category) может быть расширен администратором (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).
Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
В каждой категории есть произведения: книги, фильмы или музыка. Например, в категории «Книги» могут быть произведения «Винни-Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха.
Произведению может быть присвоен жанр (Genre) из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.
Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы (Review) и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число). На одно произведение пользователь может оставить только один отзыв.
## Установка

- Пример заполнения .env(должен находиться в каталоге infra):

     DB_ENGINE=django.db.backends.postgresql
     DB_NAME=%имя базы% 
     POSTGRES_USER=%имя пользователя% 
     POSTGRES_PASSWORD=%пароль% 
     DB_HOST=db 
     DB_PORT=%порт(5432 по умолчанию)% 

- Перейти в папку infra и запустить docker-compose.yaml (при установленном и запущенном Docker)
 ``` cd infra_sp2/infra  ```
 ``` docker-compose up  ```

- Для пересборки контейнеров выполнять команду: (находясь в папке infra, при запущенном Docker)
 ``` docker-compose up -d --build  ```

- В контейнере web выполнить миграции:
 ```docker-compose exec web python manage.py migrate ```

- Создать суперпользователя:
 ``` docker-compose exec web python manage.py createsuperuser  ```

- Собрать статику:
 ```docker-compose exec web python manage.py collectstatic --no-input ```
- Заполнение бд из файла фикстур:
 ```docker exec -it %container_id%  python manage.py shell ```
 ```>>> from django.contrib.contenttypes.models import ContentType ```
 ```>>> ContentType.objects.all().delete() ```
 ```>>> quit() ```
 ```docker-compose exec web python manage.py loaddata fixtures.json ```
-Проверьте работоспособность приложения, для этого перейдите на страницу:
  ```http://localhost/admin/ ```

## Примеры
Пользователь аутентифицируется посредством сервиса Simple JWT.
* Получите код подтверждения регистрации.
* Отправьте POST запрос с именем пользователя и e-mail на эндпойнт:

http://127.0.0.1:8000/api/v1/auth/signup/

* Получите токен для доступа к функциям сервиса проекта. 
Отправьте POST запрос с именем пользователя и полученным по e-mail кодом подтвреждения на эндпойнт:

http://127.0.0.1:8000/api/v1/auth/token/

* Можно заполнить пустые поля в своем профайле, отправив PATCH запрос на эндпойнт:

http://127.0.0.1:8000/api/v1/users/me/

* Получение списка произведений GET запрос:

http://127.0.0.1:8000/api/v1/titles/

* Получение информации о произведении GET запрос:

http://127.0.0.1:8000/api/v1/titles/{titles_id}/

* Создать отзыв о произведении POST запрос:

http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/

### В проекте использованы технологии:
- Python
- Django
- Django REST Framework
- Simple JWT
- posgresql
- docker

### Проект выполнили студенты 34 когорты Яндекс Практикума:
- Алексей Осинов   https://github.com/Lagmas
- Надежда Ткаченко https://github.com/tkachenkonn
- Юлия Галиева     https://github.com/Yuliya0506
