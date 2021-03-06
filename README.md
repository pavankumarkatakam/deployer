# Deployer
A Docker project for php Deployer https://github.com/REBELinBLUE/deployer, Which is a very nice tool for deploying php 
applications expescially Laravel application.

> Note: This project is in Development so things can be changed.

# How to use this
I recomend to use with docker-compose which is tool for composing the docker containers in a yaml file. You can learn more 
about docker compose in this [page](https://docs.docker.com/compose/compose-file/)

Sample docker-compose.yml

````
version: "2"

services:
  db:
    image: mysql:5.6
    environment:
      - MYSQL_ROOT_PASSWORD=toor
      - MYSQL_DATABASE=deployerdb
    // If you want to persist data across container start and stop
    // volumes:
    //  - hostFolder:/var/lib/mysql

  php_deployer:
    image: katakampavankumar/deployer:latest
    // If you want to persist data across container start and stop
    //volumes: 
    // - hostFolder1:/data
    ports:
      - 8081:80
    expose:
      - 9000
    links:
      - db:db
    environment:
      - DB_TYPE=mysql
      - DB_HOST=db
      - DB_DATABASE=deployerdb
      - DB_USERNAME=root
      - DB_PASSWORD=toor
      - CACHE_DRIVER=redis
      - QUEUE_DRIVER=redis
      - APP_KEY=SomeRandomStringSomeRandomString
    volumes:
      - /var/www
````
Points to remember when using this docker file
  1. Environment variables which you will give at .env file shold be given as docker environment variables.
  2. If it is first time you are running, you must ssh in to container and migrate the database.
     To do so  `docker exec -it {docker_container_name} bash` and then `php artisan migrate`.
     
# Installed items in this Docker build
   1. PHP5-fpm 
   2. nginx
   3. redis (we can use seperate container if wanted but in future)
   4. supervisor for monitoring
