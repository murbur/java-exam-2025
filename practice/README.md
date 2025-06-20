# Задание №3

Реализовать endpoint в TaskController, которому при запросе передаётся boostrapServers и название топика. В endpoint создаётся consumer Kafka, который читает топик, начиная с самых старых сообщений. Далее в Redis находим строку, по ключчу первого сообщения из Kafka ("test") и возвращаем в endpoint значение из Redis

## Решение

[Ссылка на файл TaskController.java](src/main/java/org/example/controller/TaskController.java)

При запросе на эндпоинт `/task/read`:
- из Kafka читается первое сообщение с ключом `"test"`
- затем из Redis извлекается значение по ключу `"test"`
- это значение возвращается в HTTP-ответе

Дополнительно, при старте приложения автоматически:
- создается топик Kafka `my-topic`, если он ещё не существует
- в Kafka отправляется сообщение с ключом `"test"`
- в Redis записывается значение `"hello-from-redis"`

## Как запустить

1. Запустить инфраструктуру Kafka и Redis:
```bash
docker-compose up -d
```

### 3. Запустить Spring Boot приложение
В IntelliJ открыть и запустить `Main.java`.

### 4. Проверить результат
Перейти в браузере:
```http
http://localhost:8080/task/read?bootstrapServers=localhost:9092&topic=my-topic
```
Ответ:
```text
hello-from-redis
```
