

YaMDb API

Групповой проект.

Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles). Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Category) может быть расширен администратором (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).
Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
В каждой категории есть произведения: книги, фильмы или музыка. Например, в категории «Книги» могут быть произведения «Винни-Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха.
Произведению может быть присвоен жанр (Genre) из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.
Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы (Review) и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число). На одно произведение пользователь может оставить только один отзыв.

Пользовательские роли
Аноним — может просматривать описания произведений, читать отзывы и комментарии.
Аутентифицированный пользователь (user) — может читать всё, как и Аноним, может публиковать отзывы и ставить оценки произведениям (фильмам/книгам/песенкам), может комментировать отзывы; может редактировать и удалять свои отзывы и комментарии, редактировать свои оценки произведений. Эта роль присваивается по умолчанию каждому новому пользователю.
Модератор (moderator) — те же права, что и у Аутентифицированного пользователя, плюс право удалять и редактировать любые отзывы и комментарии.
Администратор (admin) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
Суперюзер Django должен всегда обладать правами администратора, пользователя с правами admin. Даже если изменить пользовательскую роль суперюзера — это не лишит его прав администратора. Суперюзер — всегда администратор, но администратор — не обязательно суперюзер.

Самостоятельная регистрация новых пользователей
Пользователь отправляет POST-запрос с параметрами email и username на эндпоинт /api/v1/auth/signup/.
Сервис YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на указанный адрес email.
Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен).
В результате пользователь получает токен и может работать с API проекта, отправляя этот токен с каждым запросом.
После регистрации и получения токена пользователь может отправить PATCH-запрос на эндпоинт /api/v1/users/me/ и заполнить поля в своём профайле (описание полей — в документации).
Создание пользователя администратором
Пользователя может создать администратор — через админ-зону сайта или через POST-запрос на специальный эндпоинт api/v1/users/ (описание полей запроса для этого случая — в документации). В этот момент письмо с кодом подтверждения пользователю отправлять не нужно.
После этого пользователь должен самостоятельно отправить свой email и username на эндпоинт /api/v1/auth/signup/ , в ответ ему должно прийти письмо с кодом подтверждения.
Далее пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен), как и при самостоятельной регистрации.

Ресурсы API YaMDb и доступ к ним. ()

Ресурс auth: аутентификация.
Регистрация нового пользователя: http://127.0.0.1:8000/api/v1/auth/signup/

Получение JWT- токена: http://127.0.0.1:8000/api/v1/auth/token/

Ресурс users: пользователи.
Получение списка пользователей: http://127.0.0.1:8000/api/v1/users/

Добавление пользователя: http://127.0.0.1:8000/api/v1/users/

Получение пользователя по username: http://127.0.0.1:8000/api/v1/users/{username}/

Изменение данных пользователя по username: http://127.0.0.1:8000/api/v1/users/{username}/

Удаление пользователя по username: http://127.0.0.1:8000/api/v1/users/{username}/

Получение данных своей учётной записи: http://127.0.0.1:8000/api/v1/users/me/

Изменение данных своей учётной записи: http://127.0.0.1:8000/api/v1/users/me/

Ресурс titles: произведения, к которым пишут отзывы (определённый фильм, книга или песенка).

Получение списка всех произведений: http://127.0.0.1:8000/api/v1/titles/

Добавление произведения: http://127.0.0.1:8000/api/v1/titles/

Получение информации о произведении: http://127.0.0.1:8000/api/v1/titles/{titles_id}/

Частичное обновление инфорации о произведении: http://127.0.0.1:8000/api/v1/titles/{titles_id}/

Удаление произведения: http://127.0.0.1:8000/api/v1/titles/{titles_id}/

Ресурс categories: категории (типы) произведений («Фильмы», «Книги», «Музыка»).

Получение списка всех категорий: http://127.0.0.1:8000/api/v1/categories/

Добавление новой категории: http://127.0.0.1:8000/api/v1/categories/

Удаление категории: http://127.0.0.1:8000/api/v1/categories/{slug}/

Ресурс genres: жанры произведений. Одно произведение может быть привязано к нескольким жанрам.

Получение списка всех жанров: http://127.0.0.1:8000/api/v1/genres/

Добавление жанра: http://127.0.0.1:8000/api/v1/genres/

Удаление жанра: http://127.0.0.1:8000/api/v1/genres/{slug}/

Ресурс reviews: отзывы на произведения. Отзыв привязан к определённому произведению.

Получение списка всех отзывов: http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/

Добавление нового отзыва: http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/

Получение отзыва по id: http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/

Частичное обновление отзыва по id: http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/

Удаление отзыва по id: http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/

Ресурс comments: комментарии к отзывам. Комментарий привязан к определённому отзыву.

Получение списка всех комментариев к отзыву: http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/  

Добавление комментария к отзыву: http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/

Получение комментария к отзыву: http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/{comment_id}/

Частичное обновление комментария к отзыву: http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/{comment_id}/

Удаление комментария к отзыву: http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/{comment_id}/

