# Задание 2. Шардирование

***Важно!***
На хосте должен быть установлен docker, docker compose, make

## Запуск

* Переходим в директорию задания, собираем образ приложения, запускаем контейнеры

```sh
cd mongo-sharding
docker compose build --no-cache
docker compose up -d 
```

* Инициализируем кластер

```sh
make init-cluster
```

* Генерируем тестовые данные

```sh
make dummy-data
```

* Проверяем, что бэк видит тестовые данные

```sh
curl -s http://localhost:8080/helloDoc/count | jq -r .items_count
```