# docker-compose

These are the instructions to start the whole project using docker-compose

### Create network (ignore if the network was already created)

```bash
$ docker network create springcloud
```

### Setup

You need to get MySQL, Postgres and RabbitMQ running on Docker:

```bash
$ docker pull mysql:8

$ docker run -d -p 3306:3306 --name microservices-mysql8 --network springcloud \
-e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=db_springboot_cloud mysql:8

$ docker pull postgres:12-alpine

$ docker run -d -p 5432:5432 --name microservices-postgres12 --network springcloud \
-e POSTGRES_PASSWORD=root -e POSTGRES_DB=db_springboot_cloud postgres:12-alpine

$ docker pull rabbitmq:3.8-management-alpine

$ docker run -d -p 15672:15672 -p 5672:5672 --name microservices-rabbitmq38 \
--network springcloud rabbitmq:3.8-management-alpine
```

After generating the needed jar files from **springboot-service-commons-users-daos** and **springboot-service-commons**, follow the next commands to start the project:

### Start this project with docker-compose

```bash
$ docker-compose up -d config-server
$ docker-compose up -d service-eureka-server
$ docker-compose up -d service-items
$ docker-compose up -d service-users
$ docker-compose up -d service-oauth
$ docker-compose up -d service-zuul-server
$ docker-compose up -d zipkin-server
```

### Starting MySQL, Postgres and RabbitMQ with docker-compose (in case they haven't been started yet)

```bash
$ docker-compose up -d microservices-rabbitmq38
$ docker-compose up -d microservices-mysql8
$ docker-compose up -d microservices-postgres12
```

### Execute REST API
Inside this project you can find a Postman file to import into the client or use the given data to use any other client.
You must firstly get a new token from oauth project which you have to set as Authorization bearer token.