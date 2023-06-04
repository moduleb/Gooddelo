# Приложение Gooddelo

Приложение Gooddelo выдает токен доступа при регистрации или авторизации, возвращает данные о зарплате и дате повышения при предъявлении токена.

Генерирует случайные данные о зарплате и повышении в момент регистрации, сохраняет в базу данных Postgres, развернутую в Docker контейнере.
Данные базы данных хранятся в папке `pg_data` и сохраняются при удалении контейнера. Доступ к базе открыт для других приложений.




## Запуск приложения

### На локальной машине:

1. Клонировать проект с Github
2. Перейти в папку проекта
3. Запустить приложение Docker
4. Создать образ:
<br>`docker compose build`
5. Запустить контейнер:
<br>`docker compose up -d`
6. Остановить контейнер:
<br>`docker compose stop`

### На удаленном сервере:
1. Создаем папку для приложения:
<br>`mkdir gooddelo`
2. Переходим в эту папку:
<br>`cd gooddelo`
3. Скачиваем файл <b> 'docker-compose.yml'</b>: 
<br>`wget -O docker-compose.yaml https://raw.githubusercontent.com/ModuleB/gooddelo/master/docker-compose.yaml`
4. Скачиваем файл <b> 'Dockerfile'</b>: 
<br>`wget -O Dockerfile https://raw.githubusercontent.com/ModuleB/gooddelo/master/Dockerfile`
5. Скачиваем docker образы:
<br>`docker compose pull`
6. Запустить контейнер:
<br>`docker compose up -d`
7. Останить контейнер:
<br>`docker compose stop`



## Эндпоинты:

Приложение доступно по адресу:
- на локальной машине http://0.0.0.0/:8000
- на удаленном сервере http://<IP адрес сервера>:8000

<br> Информация об эндпоинтах также доступна в Swagger по адресу <b>/docs</b>

### /register <font color = gray, size = 3>[post]</font>

Принимает JSON с данными нового пользователя:
```
{
  "username": "string",
  "password": “String1”
}
```

Возвращает токен доступа или ошибку если пользователь уже существует или данные не прошли валидацию:
```
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InN0cmlmbmciLCJleHAiOjE2ODU2OTAxNzd9.bn_523efN3TdqgU1gAZzVn-RHkEMxGL3NpcHH0YTHM4",
  "token_type": "bearer"
}
```

### /login [post]:

Принимает JSON с данными уже зарегистрированного пользователя:
```
{
  "username": "string",
  "password": “String1”
}
```

### /logout [post]:
Ожидает токен доступа в заголовке ‘Authorization’
Помечает токен недействительным, дальнейшая авторизация с ним невозможна.

### /tasks

Во всех методах требуется токен доступа в заголовке ‘Authorization’
```
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InN0cmlmbmciLCJleHAiOjE2ODU2OTAxNzd9.bn_523efN3TdqgU1gAZzVn-RHkEMxGL3NpcHH0YTHM4
```
### /tasks [get]
Возвращает JSON с информацией обо всех задачах.
```
[
    {
        "id": 1,
        "name": "taskname",
        "price": 100.0,
        "creation_date": "2023-06-04T06:59:55.105448"
    },
    {
        "id": 2,
        "name": "taskname1",
        "price": 100.0,
        "creation_date": "2023-06-04T06:59:55.105448"
    }
]
```

### /tasks/{task_id} [get]
Возвращает JSON с информацией о задаче с полученным id.
```
{
    "id": 1,
    "name": "taskname",
    "price": 100.0,
    "creation_date": "2023-06-04T06:59:55.105448"
}
```


### /tasks/{task_id} [post]
Создает новую задачу. Ожидает JSON с данными:
```
{
  "name": "taskname",
  "price": “100”
}
```

### /tasks/{task_id} [put]
Обновляет информацию о задаче с полученным id.
<br>Ожидает JSON с данными:
```
{
  "name": "taskname",
  "price": “100”
}
```

### /tasks/{task_id} [delete]
Удаляет задачу с полученным id.

## База данных доступна вне приложения. Параметры подключения:
- user: <b>gooddelo</b>
- password: <b>gooddelo</b>
- name: <b>gooddelo</b>
- port: <b>5435</b>
- host:
  * на локальной машине: <b>127.0.0.1</b>
  * на удаленном сервере: <b>< IP адрес сервера ></b>
