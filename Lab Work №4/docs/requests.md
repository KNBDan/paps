# Лабораторная работа №4
## Документация по API

Базовый URL: `http://localhost:8082`

Ниже приведены все реализованные запросы с описанием параметров и примерными телами ответов.

## Пользователи

### POST /users
Описание: добавить пользователя.

Request body (application/json):
```json
{
  "tgId": 123456789,
  "fullName": "Павел",
  "course": 1,
  "program": "РИС",
  "email": "example@ex.pl"
}
```

Ответ 200 (application/json):
```json
{
  "userId": 1,
  "fullName": "Павел",
  "role": "USER",
  "course": 1,
  "program": "РИС",
  "email": "example@ex.pl",
  "isCompleteConference": false
}
```

### GET /users/{telegramId}
Описание: получить данные пользователя.

Path параметры:
- `telegramId` (integer)

Ответ 200 (application/json):
```json
{
  "userId": 1,
  "fullName": "Павел",
  "role": "USER",
  "course": 1,
  "program": "РИС",
  "email": "example@ex.pl",
  "isCompleteConference": true
}
```

### GET /users/{telegramId}/qr
Описание: получить QR-код пользователя.

Path параметры:
- `telegramId` (integer)

Ответ 200: `image/png` (бинарные данные)

### POST /users/{telegramId}/survey
Описание: отметить, что пользователь прошел опрос.

Path параметры:
- `telegramId` (integer)

Ответ 200: без тела

## Посещения

### POST /users/{telegramId}/visits/{code}
Описание: отметить посещение активности/компании по коду.

Path параметры:
- `telegramId` (integer)
- `code` (string)

Ответ 200 (application/json):
```json
{
  "target": {
    "id": 10,
    "name": "Rostics",
    "description": "Вкусная курочка каждый день",
    "siteUrl": "https://rostics.example"
  },
  "targetType": "COMPANY"
}
```
Примечание: `target` может быть компанией или активностью (в этом случае вернутся поля активности).

### GET /users
Описание: получить Telegram ID пользователей.

Query параметры:
- `size` (integer) - размер страницы.
- `token` (integer) - токен/курсора для следующей страницы.

Ответ 200 (application/json):
```json
{
  "data": [123456789, 987654321],
  "nextToken": 987654321
}
```

### GET /users/{telegramId}/visits
Описание: получить список посещений пользователя.

Path параметры:
- `telegramId` (integer)

Ответ 200 (application/json):
```json
[
  {
    "target": {
      "id": 10,
      "name": "Rostics",
      "description": "Вкусная курочка каждый день",
      "siteUrl": "https://rostics.example"
    },
    "targetType": "COMPANY"
  },
  {
    "target": {
      "id": 5,
      "name": "Собрание",
      "description": "Важное собрание",
      "activityType": "LECTURE",
      "location": "Актовый зал",
      "startTime": "10:00",
      "endTime": "11:30",
      "points": 15,
      "hasEvent": true
    },
    "targetType": "ACTIVITY"
  }
]
```

## Задания

### GET /tasks
Описание: получить все задания.

Ответ 200 (application/json):
```json
[
  {
    "id": 1,
    "type": "TEMP",
    "duration": "PT5M",
    "name": "Видео",
    "status": "READY",
    "description": "Подпрыгни три раза на видео",
    "points": 15
  }
]
```

### POST /tasks
Описание: создать задание.

Request body (application/json):
```json
{
  "name": "Видео",
  "type": "TEMP",
  "description": "Подпрыгни три раза на видео",
  "points": 15,
  "duration": 3
}
```

Ответ 200 (application/json):
```json
{
  "id": 1,
  "type": "TEMP",
  "duration": "PT3M",
  "name": "Видео",
  "status": "READY",
  "description": "Подпрыгни три раза на видео",
  "points": 15
}
```

### GET /tasks/{taskId}
Описание: получить задание по id.

Path параметры:
- `taskId` (integer)

Ответ 200 (application/json):
```json
{
  "id": 1,
  "type": "TEMP",
  "duration": "PT5M",
  "name": "Видео",
  "status": "READY",
  "description": "Подпрыгни три раза на видео",
  "points": 15
}
```

### POST /tasks/{taskId}/submit/{telegramId}
Описание: выполнить задание.

Path параметры:
- `taskId` (integer)
- `telegramId` (integer)

Ответ 200 (application/json):
```json
{
  "id": 1,
  "userId": 2,
  "taskId": 3,
  "completeTime": "10.10.2007 12:22"
}
```

### POST /tasks/{taskId}/status
Описание: изменить статус задания.

Path параметры:
- `taskId` (integer)

Request body (application/json):
```json
{
  "status": "RUN"
}
```

Ответ 200 (application/json):
```json
{
  "id": 1,
  "type": "TEMP",
  "duration": "PT5M",
  "name": "Видео",
  "status": "IN_PROCESS",
  "description": "Подпрыгни три раза на видео",
  "points": 15
}
```

### POST /tasks/all/status
Описание: изменить статус всех заданий (только завершение).

Request body (application/json):
```json
{
  "status": "END"
}
```

Ответ 200: без тела

## Задания пользователя

### GET /users/{telegramId}/tasks
Описание: получить задания пользователя.

Path параметры:
- `telegramId` (integer)

Ответ 200 (application/json):
```json
[
  {
    "id": 1,
    "name": "Видео",
    "description": "Подпрыгни три раза на видео",
    "isAvailable": true,
    "status": "IN_PROGRESS",
    "taskType": "BE_REAL"
  }
]
```

### GET /users/{telegramId}/tasks/{taskId}
Описание: получить конкретное задание пользователя.

Path параметры:
- `telegramId` (integer)
- `taskId` (integer)

Ответ 200 (application/json):
```json
{
  "id": 1,
  "name": "Видео",
  "description": "Подпрыгни три раза на видео",
  "isAvailable": true,
  "status": "DONE",
  "taskType": "BASIC_TASK"
}
```

## Предрегистрация

### POST /preregistration/users
Описание: добавить пользователя в предрегистрацию.

Request body (application/json):
```json
{
  "tgId": 123456789
}
```

Ответ 200 (application/json):
```json
{
  "tgId": 123456789
}
```

### GET /preregistration/users/{telegramId}
Описание: получить данные предрегистрации.

Path параметры:
- `telegramId` (integer)

Ответ 200 (application/json):
```json
{
  "tgId": 123456789
}
```

## Компании

### POST /companies
Описание: добавить компанию.

Request body (application/json):
```json
{
  "name": "Rostics",
  "description": "Вкусная курочка каждый день",
  "siteUrl": "https://rostics.example"
}
```

Ответ 200 (application/json):
```json
{
  "id": 1,
  "name": "Rostics",
  "description": "Вкусная курочка каждый день",
  "siteUrl": "https://rostics.example"
}
```

### GET /companies/{companyId}
Описание: получить информацию о компании.

Path параметры:
- `companyId` (integer)

Ответ 200 (application/json):
```json
{
  "id": 1,
  "name": "Rostics",
  "description": "Вкусная курочка каждый день",
  "siteUrl": "https://rostics.example"
}
```

### PUT /companies/{companyId}
Описание: обновить информацию о компании.

Path параметры:
- `companyId` (integer)

Request body (application/json):
```json
{
  "name": "Rostics",
  "description": "Стрипсы тоже есть",
  "siteUrl": "https://rostics.example"
}
```

Ответ 200 (application/json):
```json
{
  "id": 1,
  "name": "Rostics",
  "description": "Стрипсы тоже есть",
  "siteUrl": "https://rostics.example"
}
```

### DELETE /companies/{companyId}
Описание: удалить компанию.

Path параметры:
- `companyId` (integer)

Ответ 200: без тела

## Активности

### GET /activities
Описание: получить все активности.

Ответ 200 (application/json):
```json
[
  {
    "id": 1,
    "name": "Собрание",
    "description": "Важное собрание",
    "activityType": "LECTURE",
    "location": "Актовый зал",
    "startTime": "10:00",
    "endTime": "11:30",
    "hasEvent": true
  }
]
```

### POST /activities
Описание: добавить новую активность.

Request body (application/json):
```json
{
  "name": "Собрание",
  "description": "Важное собрание",
  "location": "Актовый зал",
  "type": "WORKSHOP",
  "startTime": "10:00",
  "endTime": "10:30",
  "keyWord": "Веном",
  "points": 15
}
```

Ответ 200 (application/json):
```json
{
  "id": 1,
  "name": "Собрание",
  "description": "Важное собрание",
  "activityType": "WORKSHOP",
  "location": "Актовый зал",
  "startTime": "10:00",
  "endTime": "10:30",
  "hasEvent": false
}
```

### GET /activities/{activityId}
Описание: получить информацию об активности.

Path параметры:
- `activityId` (integer)

Ответ 200 (application/json):
```json
{
  "id": 1,
  "name": "Собрание",
  "description": "Важное собрание",
  "activityType": "LECTURE",
  "location": "Актовый зал",
  "startTime": "10:00",
  "endTime": "11:30",
  "hasEvent": true
}
```

### POST /activities/{activityId}/visit/{userCode}
Описание: отметить посещение активности участником.

Path параметры:
- `activityId` (integer)
- `userCode` (string)

Ответ 200 (application/json):
```json
{
  "id": 1,
  "name": "Собрание",
  "description": "Важное собрание",
  "activityType": "LECTURE",
  "location": "Актовый зал",
  "startTime": "10:00",
  "endTime": "11:30",
  "hasEvent": true
}
```

### POST /activities/{activityId}/visit-copy/{toActivityId}
Описание: скопировать посещения активности.

Path параметры:
- `activityId` (integer)
- `toActivityId` (integer)

Ответ 200: без тела

### POST /activities/key-word
Описание: отправить кодовое слово от пользователя.

Request body (application/json):
```json
{
  "tgId": 123456789,
  "keyWord": "Какашка"
}
```

Ответ 200: без тела

## Ивенты активности

### GET /activities/{activityId}/event
Описание: получить информацию об ивенте.

Path параметры:
- `activityId` (integer)

Ответ 200 (application/json):
```json
{
  "id": 1,
  "name": "Голосование",
  "description": "Выберите лучший проект",
  "duration": "PT10M",
  "status": "ENDED",
  "answers": ["Один", "Два", "Три"]
}
```

### POST /activities/{activityId}/event
Описание: добавить новый ивент к активности.

Path параметры:
- `activityId` (integer)

Request body (application/json):
```json
{
  "name": "Голосование",
  "description": "Выберите лучший проект",
  "duration": "PT5M",
  "answers": ["Один", "Два", "Три"],
  "rightAnswer": "Один",
  "reward": 100
}
```

Ответ 200 (application/json):
```json
{
  "id": 1,
  "name": "Голосование",
  "description": "Выберите лучший проект",
  "duration": "PT5M",
  "status": "PREPARED",
  "answers": ["Один", "Два", "Три"]
}
```

### POST /activities/{activityId}/event/status
Описание: обновить статус ивента.

Path параметры:
- `activityId` (integer)

Request body (application/json):
```json
{
  "status": "RUN"
}
```

Ответ 200: без тела

### POST /activities/{activityId}/event/answer
Описание: добавить ответ пользователя.

Path параметры:
- `activityId` (integer)

Request body (application/json):
```json
{
  "userTgId": 123456789,
  "answer": "Один"
}
```

Ответ 200 (application/json):
```json
{
  "id": 1,
  "userId": 2,
  "answer": "Один",
  "eventId": 4
}
```

## Роли и авторизация

### POST /auth/code/generate
Описание: получить коды новых ролей.

Request body (application/json):
```json
{
  "count": 3,
  "type": "ADMIN"
}
```

Ответ 200 (application/json):
```json
["FOSfWRSGEq", "K2l3X9PqLm", "Zx7Vt1QwEr"]
```

### POST /auth/code/activate
Описание: добавить роль пользователю.

Request body (application/json):
```json
{
  "tgId": 123456789,
  "code": "FOSfWRSGEq"
}
```

Ответ 200: без тела
