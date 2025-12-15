# Задание 3. Репликация

***Важно!***
На хосте должен быть установлен docker, docker compose, make

## Запуск

* Переходим в директорию задания, собираем образ приложения, запускаем контейнеры

```sh
cd mongo-sharding-repl
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

* Проверяем, что кэширование работает

```sh
# Первый запрос
curl -s -o /dev/null -w "@format.txt" http://localhost:8080/helloDoc/users
URL: http://localhost:8080/helloDoc/users 
DNS lookup time: 0.000016  seconds
Connect time: 0.000084  seconds
Start transfer time: 1.044826  seconds
HTTP code: 200 
Total time: 1.057362 seconds

# Последующие запросы
curl -s -o /dev/null -w "@format.txt" http://localhost:8080/helloDoc/users
URL: http://localhost:8080/helloDoc/users 
DNS lookup time: 0.000036  seconds
Connect time: 0.000218  seconds
Start transfer time: 0.015422  seconds
HTTP code: 200 
Total time: 0.015548 seconds

curl -s -o /dev/null -w "@format.txt" http://localhost:8080/helloDoc/users
URL: http://localhost:8080/helloDoc/users 
DNS lookup time: 0.000021  seconds
Connect time: 0.000111  seconds
Start transfer time: 0.007425  seconds
HTTP code: 200 
Total time: 0.007607 seconds
```