# Чистый проект Symfony 6 c DOCKER. 
## В Docker влкючены:
- WEB
- MYSQL
- REDIS

### Собираем локально Docker
```bash
docker-compose -f docker/docker-compose.yml -p yourprojectname build
```

### Запускаем локально
```bash
docker-compose -f docker/docker-compose.yml -p yourprojectname up
```

# Войти в Docker:
```bash
docker container ls
docker exec -it yourprojectname-web-1 bash
docker exec -it yourprojectname-db-1 bash
docker exec -it yourprojectname-rediscache-1 redis-cli
```

# войти в Mysql
```bash
mysql -uroot -p --port=3308
```

### Работа с Redis
```
DBSIZE // размер базы Redis
KEYS * // список ключей
LRANGE 192_37980766082_1 0 100 // доступ к данным по ключу
GET 11029_66652539728_1_2563   // доступ к данным по ключу
FLUSHDB // удалит все из базы 
```