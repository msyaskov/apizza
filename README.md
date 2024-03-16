# Apizza
Приложение по заказу пицц.

## Логирование в ELK
Для того чтобы подключить логи вашего сервиса к ELK, выполните следующие действия:
* добавьте зависимость `net.logstash.logback:logstash-logback-encoder:7.4`
* добавьте в `application.properties` свойство `logstash.host=localhost:5030`
* добавьте в `application.properties` свойство `spring.application.name`
* добавьте в `resources` файл `logback-spring.xml` со следующим содержимым:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <include resource="org/springframework/boot/logging/logback/defaults.xml"/>
    <include resource="org/springframework/boot/logging/logback/console-appender.xml"/>

    <springProperty scope="context" name="service.name" source="spring.application.name" />
    <springProperty scope="context" name="logstash.host" source="logstash.host" />

    <appender name="LOGSTASH" class="net.logstash.logback.appender.LogstashTcpSocketAppender">
        <destination>${logstash.host}</destination>
        <encoder class="net.logstash.logback.encoder.LogstashEncoder"/>
    </appender>

    <root level="INFO">
        <appender-ref ref="CONSOLE" />
        <appender-ref ref="LOGSTASH" />
    </root>
</configuration>
```

Для просмотра логов через web-интерфейс выполните следующее:
* перейдите по адресу `http://localhost:5601`
* в меню перейдите по `Observability` -> `Logs` -> `Explorer`
* нажмите кнопку `View all matches` (если логи не появились)
* настройте `fields`, нужны `service.name` и `message` 

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