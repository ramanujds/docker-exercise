# Docker Exercise

__What are docker image and docker containers?__

>Docker Images are Templates used to create Docker Container
>Container is a running instance of an image

## Basic Commands

### Version

```bash
docker -v
```

## Docker Images

### List of Images

```bash
docker images
```

### List of commands for images

```bash
docker images --help
```

### Pull an image

```bash
docker pull ubuntu
```

### Filtering images

```bash
docker images -f "dangling=true"
```

>Dangling images are images which are associated with any containers

### Running an image

```bash
docker run --name MyUbuntu -it ubuntu bash
```

### Inspecting an image

```bash
docker inspect ubuntu
```

### Remove an Image

```bash
docker rmi ubuntu
```

## Docker Containers

### Show the list of running containers

```bash
docker ps
```

### Show the list of all containers

```bash
docker ps -a
```

### Start a container

```bash
docker start <container name/id>
```

### Stop a container

```bash
docker stop <container name/id>
```

### Pause a container

```bash
docker pause <container name/id>
```

### Resume a container

```bash
docker unpause <container name/id>
```

### Displaying stats

```bash
docker stats <container name/id>
```

### Remove a container

```bash
docker rm <container name/id>
```

### Get the history of an image

```bash
docker history <image name/id>
```

## Docker Volume

## Docker Mysql + SpringBoot

## How to run MySql on Docker -
  
  ```bash
   docker pull mysql
   docker run -d -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=mydb -e MYSQL_USER=docker -e MYSQL_PASSWORD=password -p 3308:3306 --name mysql mysql:5.7
  ```

## Running Spring Boot 3 Projects on Docker

### Creating image through Spring Boot -
  
  ```javascript
  mvn spring-boot:build-image
  ```

### Running docker image -

* Link Based Communication -
  
  ```bash
  docker container run -p 5000:5000 -e RDS_HOSTNAME=mysql -e RDS_PORT=3306 --link=mysql --name book-store book-store-server:0.0.1-SNAPSHOT
  ```

* Host Based Communication -
  
```bash
  docker container run -p 5000:5000 --network=host book-store-server:0.0.1-SNAPSHOT
```

* Custom Network

  * Creating a Custom Network :

```bash
docker network create book-store-mysql-network
```

* Running Mysql Container on that network -

```bash
docker run -d -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=mydb -e MYSQL_USER=docker -e MYSQL_PASSWORD=password -p 3308:3306 --name mysql 
--network=book-store-mysql-network mysql:5.7
```

* Running book-store on that network -

```bash
docker container run -p 5000:5000 -e RDS_HOSTNAME=mysql -e RDS_PORT=3306 --network=book-store-mysql-network -d --name book-store book-store-server:0.0.1-SNAPSHOT
```

## Using Docker volume to Persist Data - 

```bash
docker run -d -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=mydb -e MYSQL_USER=docker -e MYSQL_PASSWORD=password -p 3308:3306 --name mysql --volume mysql-db-volume:/var/lib/mysql mysql
```

# Running Fullstack Application (Spring Boot + MySql + Angular)
