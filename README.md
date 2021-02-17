# Docker Exercise

__What are docker image and docker containers?__

>Docker Images are Templates used to create Docker Container
>Container is a running instance of an image

## Basic Commands

* Version

```bash
docker -v
```

## Docker Images

* List of Images

```bash
docker images
```

* List of commands for images

```bash
docker images --help
```

* Pull an image

```bash
docker pull ubuntu
```

* Filtering images

```bash
docker images -f "dangling=true"
```

>Dangling images are images which are associated with any containers

* Running an image

```bash
docker run --name MyUbuntu -it ubuntu bash
```

* Inspecting an image

```bash
docker inspect ubuntu
```

* Remove an Image

```bash
docker rmi ubuntu
```

## Docker Containers

* Show the list of running containers

```bash
docker ps
```

* Show the list of all containers

```bash
docker ps -a
```

* Start a container

```bash
docker start <container name/id>
```

* Stop a container

```bash
docker stop <container name/id>
```

* Pause a container

```bash
docker pause <container name/id>
```

* Resume a container

```bash
docker unpause <container name/id>
```

* Displaying stats

```bash
docker stats <container name/id>
```

* Remove a container

```bash
docker rm <container name/id>
```

* Get the history of an image

```bash
docker history <image name/id>
```

## Dockerfile

__What is a dockerfile?__

> A Dockerfile is a simple text file with instruction to build images
> Automation of docker image creation

![Dockerfile](https://i2.wp.com/miro.medium.com/max/1273/1*p8k1b2DZTQEW_yf0hYniXw.png?w=810&ssl=1)

__How to create a Dockerfile?__

* Step 1 : Create a file named Dockerfile

* Step 2 : Add instructions in Dockerfile

* Step 3 : Build Dockerfile to create image

* Step 4 : Run image to create container

### Dockerfile example

```bash
FROM ubuntu

MAINTAINER ramanuj das <ramanujds9@gmail.com> 

RUN  apt-get update

CMD ["echo", "Hello World...! Image Running.."]

```

__How to build image from Dockerfile?__

```bash

docker build -t myUbuntu:1.0 .

#t flag is used for tagging an image

```

__Running the image:__

```bash
docker run <image id>
```


## Docker Volume

> Docker volumes are used for decoupling container from storage

__Command to create a docker volume:__

```bash
docker volume create myVolume1
```

__Running jenkins with the volume:__

```bash
docker run --name myJenkins1 -v myVolume1:/var/jenkins_home -p 9000:8080 -p 50000:50000 jenkins
```

## Docker Mysql + SpringBoot

## How to run MySql on Docker -
  
  ```bash
   docker pull mysql
   docker run -d -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=mydb -e MYSQL_USER=docker -e MYSQL_PASSWORD=password -p 3308:3306 --name mysql mysql
  ```

## Running Spring Boot 3 Projects on Docker

* Creating image through Spring Boot -
  
  ```bash
  mvn spring-boot:build-image
  ```

* Running docker image -

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
docker run -d -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=mydb -e MYSQL_USER=docker -e MYSQL_PASSWORD=password -p 3308:3306 --name mysql --network=book-store-mysql-network mysql
```

* Running book-store on that network -

```bash
docker container run -p 5000:5000 -e RDS_HOSTNAME=mysql -e RDS_PORT=3306 --network=book-store-mysql-network -d --name book-store book-store-server:0.0.1-SNAPSHOT
```

## Using Docker volume to Persist Data - 

```bash
docker run -d -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=mydb -e MYSQL_USER=docker -e MYSQL_PASSWORD=password -p 3308:3306 --name mysql --volume mysql-db-volume:/var/lib/mysql --network=book-store-mysql-network mysql
```


## Running Angular Application 

__How to run an Angular Application in Docker?__

* Build the Angular App

```bash
ng build [app-name] --prod=true
```

* Create a Dockerfile in the root directory

```Dockerfile
FROM nginx
COPY dist/[app-name] /usr/share/nginx/html
```

* Build the image

```bash
docker build -t [image-name] .
```

* Run the image

```bash
docker run -p 80:80  [image-name]
```

## Docker Compose

__What is Docker compose?__

* Tool for defining and running musti-container Docker application
* It uses yaml file to configure application services(docker-compose.yml)
* Can start all services with a single command
* Can scale up selected services when required 

## Steps

* Step 1 : Create the docker compose file (docker-compose.yml)
* Step 2 : Check the validity of the file using command:

```bash
docker-compose config
```

* Step 3 : Run docker-compose yml file using command:

```bash
docker-compose up
```

* Step 4 : Stop docker-compose using command:

```bash
docker-compose down
```

__docker-compose.yml exmaple:__

```yml

version: '3'

services:
    web:
        image: nginx
        ports:
            - 9090:80
    database:
        image: redis

```

__How to use scaling?__

```bash
docker-compose up -d --scale database=4
```

### Running Fullstack Application using Docker-Compose

```yml
version: '3.7'

services:

  book-store-frontend:
    image: book-store-client-app 
    #build:
      #context: .
      #dockerfile: Dockerfile
    ports:
      - "4200:80"
    restart: always
    depends_on: # Start the depends_on first
      - book-store-spring-boot 
    networks:
      - book-store-app-network

  book-store-spring-boot:
    image: book-store-server:0.0.1-SNAPSHOT
    #build:
      #context: .
      #dockerfile: Dockerfile
    ports:
      - "5000:5000"
    restart: always
    # depends_on: # Start the depends_on first
    #   - mysql 
    environment:
      RDS_HOSTNAME: mysql
      RDS_PORT: 3306
      RDS_DB_NAME: mydb
      RDS_USERNAME: docker
      RDS_PASSWORD: password
    networks:
      - book-store-app-network

  mysql:
    image: mysql
    command: --default-authentication-plugin=mysql_native_password
    ports:
      - "3308:3306"
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_USER: docker
      MYSQL_PASSWORD: password
      MYSQL_DATABASE: mydb
      MYSQL_ROOT_HOST: 192.168.0.48/255.255.255.248
    # volumes:
    #   - mysql-db-volume:/var/lib/mysql
    networks:
      - book-store-app-network
      # network:
      #   ipv4_address: 172.20.0.2
  
# Volumes
# volumes: 
#   mysql-db-volume:

networks:
  book-store-app-network:

```

### Some useful Docker Compose Commands

* Docker-compose Configuration

```bash
docker-compose config
```

* Docker-Compose Images Used

```bash
docker-compose images
```

* Docker-Compose Container Used

```bash
docker-compose ps
```

* Docker-Compose Pause, Resume, Stop, Kill 

```bash
docker-compose pause
docker-compose unpause
docker-compose stop
docker-compose kill

```


* Docker-Compose Events 

```bash
docker-compose events
```


* Build Images from Dockerfile 

```bash
docker-compose build

```

# Running Microservices on Docker
