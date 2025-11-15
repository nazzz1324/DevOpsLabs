#Лабораторная работа №2 по теме "Docker: создание и управление контейнерами"

## 1. Цель работы

Целью работы является изучение работы с программным обеспечением Docker для автоматизации развертывания и управления приложениями в средах с поддержкой контейнеризации.

##Ход работы

Клонировали репозиторий данный преподавателем. Создали и заполнили файл Dockerfile 
<img width="1557" height="672" alt="image" src="https://github.com/user-attachments/assets/1f066e16-4a90-4f3d-a790-1e6ffe8353ce" />

Запустили докер контейнер на порте 1234
<img width="1236" height="593" alt="image" src="https://github.com/user-attachments/assets/e401270e-4419-4023-95f8-0e2daef8d837" />
<img width="1543" height="122" alt="image" src="https://github.com/user-attachments/assets/d5e2d8fa-5029-4fd6-9cc0-185eca1c2523" />
<img width="1919" height="792" alt="image" src="https://github.com/user-attachments/assets/ffb9eb5c-d281-4ac2-9bfd-9905849a10f3" />

Написали docker-compose.yml для развертывания этого приложения вместе с базой данных PostgreSQL, предусмотреkb проброс порта 1234, а также возможность расширения или подключения сторонних сервисов при необходимости.

```
version: '3.8'

services:
  web:
    build: .
    container_name: flask_app
    ports:
      - "1234:1234"
    environment:
      - APP_PORT=1234
      - POSTGRES_HOST=db
      - POSTGRES_DB=testdb
      - POSTGRES_USER=testuser
      - POSTGRES_PASSWORD=testpass
    depends_on:
      - db
    restart: always

  db:
    image: postgres:16
    container_name: postgres_db
    environment:
      - POSTGRES_DB=testdb
      - POSTGRES_USER=testuser
      - POSTGRES_PASSWORD=testpass
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: always

volumes:
  postgres_data:
```

Запустиkb стек через docker-compose и убедиkbcm что подключение к базе данных произошло успешно.
<img width="293" height="88" alt="image (6)" src="https://github.com/user-attachments/assets/e112d6ac-a6ff-4169-83e8-98d17f4b2842" />
