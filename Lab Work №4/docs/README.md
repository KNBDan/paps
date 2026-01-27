# Лабораторная работа №4
## Тема:
Проектирование REST API
## Цель работы: 
Получить опыт проектирования программного интерфейса.

## Документация по API
В рамках работы был разработан REST API для телеграмм бота для конференции. API исопльзует HTTP-методы и формат данныъ JSON.

## Принятые решения при проектировании API
1. Именование ресурсов - используем существительные во множественном числе: `/users`, `/companies`, `/activities`, чтобы URL отражал коллекции объектов.
2. HTTP‑методы - GET для чтения, POST для создания, PUT для обновления, DELETE для удаления, чтобы поведение соответствовало стандарту REST.
3. Ответ на создание - при POST возвращается созданный объект (например, Company/Task/Activity), чтобы клиент сразу получал итоговые данные.
4. Формат данных - запросы и ответы передаются в JSON, а QR‑код пользователя отдаётся как ".png".
5. Структура URL - связанные сущности оформлены вложенными ресурсами: `/users/{telegramId}/visits`, `/users/{telegramId}/tasks`, `/activities/{activityId}/event`.
6. Отдельные URL для особых действий - в блоках `users`, `activities`, `tasks` добавлены специализированные URL (например, `survey`, `submit`, `status`, `visit-copy`), так как эти операции требуют отдельной логики и не являются обычным CRUD.
7. Единый формат ошибок - ошибки возвращаются в виде `ErrorResponse` (поля `errorType`, `message`), чтобы клиент мог одинаково обрабатывать исключения.
8. Авторизация - все запросы (кроме health‑check) требуют Basic Auth; логин/пароль задаются в конфиге, что защищает API.
9. Проверка входных данных - для полей в запросах заданы правила (не пусто, только положительное число, корректная почта/ссылка), чтобы не принимать мусорные данные.

## Описание API  
### Базовый URL:
http://localhost:8082
### Basic Auth
Логин: test
Пароль: test

## Описание API (кратко)

### Пользователи
- GET /users - получить Telegram ID пользователей
- POST /users - добавить пользователя
- GET /users/{telegramId} - получить данные о пользователе
- GET /users/{telegramId}/qr - получить QR-код пользователя
- POST /users/{telegramId}/survey - отметить прохождение опроса

### Посещения
- POST /users/{telegramId}/visits/{code} - посетить активность/компанию по коду
- GET /users/{telegramId}/visits - получить посещения пользователя

### Задания
- GET /tasks - получить все задания
- POST /tasks - создать задание
- GET /tasks/{taskId} - получить задание
- POST /tasks/{taskId}/submit/{telegramId} - выполнить задание
- POST /tasks/{taskId}/status - изменить статус задания
- POST /tasks/all/status - изменить статус всех заданий

### Задания пользователя
- GET /users/{telegramId}/tasks - получить задания пользователя
- GET /users/{telegramId}/tasks/{taskId} - получить задание пользователя

### Предрегистрация
- POST /preregistration/users - добавить пользователя
- GET /preregistration/users/{telegramId} - получить данные о пользователе

### Компании
- POST /companies - добавить компанию
- GET /companies/{companyId} - получить компанию
- PUT /companies/{companyId} - обновить компанию
- DELETE /companies/{companyId} - удалить компанию

### Активности
- GET /activities - получить все активности
- POST /activities - добавить активность
- GET /activities/{activityId} - получить активность
- POST /activities/{activityId}/visit/{userCode} - отметить посещение активности
- POST /activities/{activityId}/visit-copy/{toActivityId} - скопировать посещения активности
- POST /activities/key-word - отправить кодовое слово

### Ивенты активности
- GET /activities/{activityId}/event - получить ивент
- POST /activities/{activityId}/event - создать ивент
- POST /activities/{activityId}/event/status - обновить статус ивента
- POST /activities/{activityId}/event/answer - добавить ответ пользователя

### Роли
- POST /auth/code/generate - получить коды ролей
- POST /auth/code/activate - активировать роль пользователю

## Описание методов
Подробное описание методов в файле `requests.md`

## Тестирование методов в Postman
8 методов с тестами находятся в файле `postman_tests.md`, были протестированы следующие запросы 
### Компании
- POST /companies - добавить компанию
- GET /companies/{companyId} - получить компанию
- PUT /companies/{companyId} - обновить компанию
- DELETE /companies/{companyId} - удалить компанию
### Активности
- GET /activities/{activityId} - получить активность
- POST /activities - добавить активность
### Пользователи
- POST /users - добавить пользователя
- GET /users/{telegramId} - получить данные о пользователе