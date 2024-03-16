# Apizza
Приложение по заказу пицц.

## Deploy
Приложение разворачивается с помощью docker compose версии 2.24+.
  
Файл конфигурации находится по пути [docker/docker-compose.yml](./docker/docker-compose.yml).  
Настройки контейнеров по умолчанию:
* [postges](./docker/postgres.env):  
Порт: `5432`  
База данных: `apizza_db`  
Username: `username`  
Password: `password`  
URL: `jdbc:postgresql://localhost:5432/apizza_db`


* [kafka](./docker/kafka.env):  
URL для других контейнеров: `kafka:9092`  
URL для внешних приложений: `localhost:29092`