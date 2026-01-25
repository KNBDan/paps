# Шаблон тестирования API

Ниже структура для заполнения отчёта по тестированию.  
Для каждого эндпоинта - 2 запроса: корректный и некорректный.

## 1. POST /companies - создать компанию

### Метод: POST  
### URL:
`http://localhost:8082/companies`

### Body 1:
```json 
{
  "name": "Rostics",
  "description": "Вкусная курочка каждый день",
  "siteUrl": "https://rostics.example"
}
```
### Ответ Postman 1:
![1.1](/images/post1.1.png)

### Body 2:
```json
{
}
```
### Ответ Postman 2:
![1.2](/images/post1.2.png)

## 2. GET /companies/{companyId} - получить компанию

### Метод: GET  
### URL:
`http://localhost:8082/companies/{companyId}`

### Запрос 1 (корректный):
пример: `http://localhost:8082/companies/1`

### Ответ Postman 1:
![2.1](/images/post2.1.png)

### Запрос 2 (некорректный):
пример: `http://localhost:8082/companies/2414`

### Ответ Postman 2:
![2.2](/images/post2.2.png)

## 3. PUT /companies/{companyId} - обновить компанию

### Метод: PUT  
### URL:
`http://localhost:8082/companies/{companyId}`

### Body 1:
```json
{
  "name": "Rostics",
  "description": "Обновленное описание",
  "siteUrl": "https://rostics.example"
}
```
### Ответ Postman 1:
![3.1](/images/post3.1.png)

### URL 2: 
`http://localhost:8082/companies/1234`
### Body 2:
```json
{
  "name": "Rostics",
  "description": "Обновленное описание",
  "siteUrl": "https://rostics.example"
}
```
### Ответ Postman 2:
![3.2](/images/post3.2.png)

## 4. DELETE /companies/{companyId} - удалить компанию

### Метод: DELETE  
### URL:
`http://localhost:8082/companies/{companyId}`

### Запрос 1 (корректный):
пример: `http://localhost:8082/companies/1`

### Ответ Postman 1:
![4.1](/images/post4.1.png)

### Запрос 2 (некорректный):
пример: `http://localhost:8082/companies/999999`

### Ответ Postman 2:
![4.2](/images/post4.2.png)

## 5. POST /users - создать пользователя

### Метод: POST  
### URL:
`http://localhost:8082/users`

### Body 1:
```json
{
  "tgId": 123456789,
  "fullName": "Павел",
  "course": 1,
  "program": "РИС",
  "email": "example@ex.pl"
}
```
### Ответ Postman 1:
![5.1](/images/post5.1.png)

### Body 2:
```json
{
}
```
### Ответ Postman 2:
![5.2](/images/post5.2.png)

## 6. GET /users/{telegramId}/qr - получить QR-код пользователя

### Метод: GET  
### URL:
`http://localhost:8082/users/{telegramId}/qr`

### Запрос 1 (корректный):
пример: `http://localhost:8082/users/123456789/qr`

### Ответ Postman 1:
![6.1](/images/post6.1.png)

### Запрос 2 (некорректный):
пример: `http://localhost:8082/users/999999999/qr`

### Ответ Postman 2:
![6.2](/images/post6.2.png)

## 7. POST /activities — создать активность

### Метод: POST  
### URL:
`http://localhost:8082/activities`

### Body 1:
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
### Ответ Postman 1:
![7.1](/images/post7.1.png)

### Body 2:
```json
{
}
```
### Ответ Postman 2:
![7.2](/images/post7.2.png)

## 8. POST /activities/{activityId}/visit/{userCode} — посещение активности пользователем

### Метод: POST  
### URL:
`http://localhost:8082/activities/{activityId}/visit/{userCode}`

### Запрос 1 (корректный):
пример: `http://localhost:8082/activities/1/visit/UNUHEskUntz5F`

### Ответ Postman 1:
![8.1](/images/post8.1.png)

### Запрос 2 (некорректный):
пример: `http://localhost:8082/activities/1/visit/NOTAUSER`

### Ответ Postman 2:
![8.2](/images/post8.2.png)

### Запрос 3 (некорректный):
пример: `http://localhost:8082/activities/1234/visit/UNUHEskUntz5F`

### Ответ Postman 3:
![8.3](/images/post8.3.png)