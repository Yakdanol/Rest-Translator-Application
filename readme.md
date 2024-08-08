<div align="center">
    <h1> Translation Service </h1>
</div>

## _**Требования**_
Перед началом убедитесь, что на вашем компьютере установлены следующие компоненты:
* Java 17 или выше
* Gradle 7.0 или выше
* Доступ к интернету для взаимодействия с API Яндекс Переводчика

---

## _**Запуск приложения**_
Для запуска приложения выполните команду:
```sh
./gradlew bootRun
```
После этого приложение будет доступно по адресу http://localhost:8080.

---

## _**Запуск с использованием Docker**_
1. Убедитесь, что Docker и Docker Compose установлены на вашем компьютере.
2. Создайте файл docker-compose.yml со следующим содержимым:
```yaml
version: '3.8'

services:
  db:
    image: postgres:latest
    container_name: translation-db
    environment:
      POSTGRES_DB: translation
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
    ports:
      - "5432:5432"
    networks:
      - translation-network

  app:
    image: your-translation-service-image:latest
    container_name: translation-app
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://db:5432/translation
      SPRING_DATASOURCE_USERNAME: postgres
      SPRING_DATASOURCE_PASSWORD: password
    ports:
      - "8080:8080"
    depends_on:
      - db
    networks:
      - translation-network

networks:
  translation-network:
    driver: bridge
```
3. Запустите контейнеры с помощью команды:
```sh
docker-compose up
```
После этого приложение будет доступно по адресу http://localhost:8080.

---

## _**Использование**_
Отправка запроса на перевод:
Чтобы выполнить перевод текста, отправьте POST-запрос на эндпоинт /translate. Пример запроса:
```sh
curl -X POST http://localhost:8080/translate -H "Content-Type: application/json" -d '{
"ipAddress": "127.0.0.1",
"text": "Hello world",
"sourceLang": "en",
"targetLang": "ru"
}'
```

Пример успешного ответа:
В случае успешного перевода, вы получите ответ:
```json
{
"translatedText": "http 200 Привет мир"
}
```

Пример ответа с ошибкой:
Если возникла ошибка, например, неверно указан язык, вы получите ответ:
```json
{
"translatedText": "http 400 Не найден язык исходного сообщения"
}
```
---

## _**Общая информация**_

Служба перевода (Translation Service) - это веб-приложение, разработанное для выполнения автоматических переводов текстов с одного языка на другой с использованием API Яндекс Переводчика. Приложение поддерживает асинхронные запросы на перевод, кэширование результатов для повышения производительности и обработку ошибок.

### _**Технологии и инструменты**_
#### _**Основные технологии:**_
* Java: Основной язык программирования.
* Spring Boot: Фреймворк для создания автономных, production-ready приложений на основе Spring.
* RestTemplate: Инструмент для выполнения HTTP-запросов.
* Caffeine Cache: Библиотека для кэширования данных в памяти.

#### _**Вспомогательные инструменты:**_
* Gradle: Система автоматизации сборки.
* Lombok: Библиотека для сокращения шаблонного кода.
* Mockito: Фреймворк для создания mock-объектов в тестах.
* JUnit 5: Фреймворк для модульного тестирования.

### _**Архитектура**_
Приложение организовано по многослойной архитектуре, включающей следующие слои:
* Controller Layer: Обрабатывает входящие HTTP-запросы и возвращает ответы.
* Service Layer: Содержит бизнес-логику приложения.
* Repository Layer: Взаимодействует с базой данных для хранения и извлечения данных.
* Exception Handling: Обрабатывает исключения и возвращает соответствующие HTTP-статусы.

### _**Функциональные возможности:**_
#### _**Основные функции:**_
* Перевод текста: Перевод текста с одного языка на другой с использованием API Яндекс Переводчика.
* Асинхронная обработка: Обработка переводов асинхронно для повышения производительности.
* Кэширование переводов: Использование Caffeine Cache для хранения результатов переводов и уменьшения числа обращений к API.
* Обработка ошибок: Логирование и обработка ошибок при взаимодействии с API.

---

## _**Важно**_
## _**Работа с API**_
Для удобства запуска и тестирования приложения ключ API был использован прямо в коде. Это решение принято для упрощения использования приложения другими пользователями и быстрой проверки его работоспособности.

## _**Рекомендации для улучшения:**_
1. Использование переменных окружения: Вместо того, чтобы хардкодить ключи в коде, используйте переменные окружения. Это позволит легко менять конфигурации без изменения кода.
```yaml
environment:
  TRANSLATION_API_KEY: your_api_key
  TRANSLATION_API_URL: https://translate.yandex.net/api/v1.5/tr.json/translate
```

2. Конфигурационные файлы: Используйте конфигурационные файлы для хранения таких настроек. Например, в Spring Boot можно использовать application.properties или application.yml.
```properties
translation.api.key=${TRANSLATION_API_KEY}
translation.api.url=${TRANSLATION_API_URL}
```
