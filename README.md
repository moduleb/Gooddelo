![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![FastAPI](https://img.shields.io/badge/FastAPI-%0c584b?style=for-the-badge&logo=fastapi&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white)
![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-black?style=for-the-badge&logo=sqlalchemy&logoColor=red)
![Redis](https://img.shields.io/badge/redis-%23DD0031.svg?style=for-the-badge&logo=redis&logoColor=white)
![Poetry](https://img.shields.io/badge/Poetry-%233B82F6.svg?style=for-the-badge&logo=poetry&logoColor=0B3D8D)

# Приложение Gooddelo

Основные функции приложения:
- Выдача токена доступа при регистрации или авторизации.
- Возможность выхода из системы.
- Создание задач с полями "Имя" и "Прайс".
- Возврат данных о созданных задачах при предъявлении токена.

Технологии и инструменты, используемые в приложении:
- Использование Pydantic для верификации данных.
- Хранение NoSQL данных в Redis.
- Ограничение количества запросов с помощью Slowapi.
- Сохранение информации в базе данных Postgres.
- Использование SQLAlchemy для доступа к базе данных.
- Разворачивание приложения с помощью Docker Compose.
- Использование Docker Volume для хранение данных Postgres. Доступ к базе открыт для других приложений.

-----------------------------------------------------
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


-----------------------------------------------------
## Эндпоинты:

Приложение доступно по адресу:
- на локальной машине http://0.0.0.0/:8000
- на удаленном сервере http://<IP адрес сервера>:8000

<br> Информация об эндпоинтах также доступна в Swagger по адресу <b>/docs</b>

### 
### **[post]** .../register 

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
###
### **[post]** .../login

Принимает JSON с данными уже зарегистрированного пользователя:
```
{
  "username": "string",
  "password": “String1”
}
```
Возвращает токен доступа или ошибку если пользователя с такими данными не существует.

###
### **[post]** .../logout

Ожидает токен доступа в заголовке ‘Authorization’
<br>Помечает токен недействительным, дальнейшая авторизация с ним невозможна.


---

## /tasks

Во всех методах требуется токен доступа в заголовке ‘Authorization’
```
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InN0cmlmbmciLCJleHAiOjE2ODU2OTAxNzd9.bn_523efN3TdqgU1gAZzVn-RHkEMxGL3NpcHH0YTHM4
```

###
### **[get]** .../tasks
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

###
### **[get]** .../tasks/{task_id}
Возвращает JSON с информацией о задаче с полученным id.
```
{
    "id": 1,
    "name": "taskname",
    "price": 100.0,
    "creation_date": "2023-06-04T06:59:55.105448"
}
```

###
### **[post]** .../tasks/{task_id}
Создает новую задачу. Ожидает JSON с данными:
```
{
  "name": "taskname",
  "price": “100”
}
```

###
### [put] .../tasks/{task_id}
Обновляет информацию о задаче с полученным id.
<br>Ожидает JSON с данными:
```
{
  "name": "taskname",
  "price": “100”
}
```

###
### **[delete]** .../tasks/{task_id}
Удаляет задачу с полученным id.

---

## База данных доступна вне приложения. Параметры подключения:
- user: <b>gooddelo</b>
- password: <b>gooddelo</b>
- name: <b>gooddelo</b>
- port: <b>5435</b>
- host:
  * на локальной машине: <b>127.0.0.1</b>
  * на удаленном сервере: <b>< IP адрес сервера ></b>
