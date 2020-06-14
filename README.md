# Docker-exercise

All Docker Related Exercise

## How to run MySql on Docker -
  
  ```bash
   docker pull mysql
   docker run -d -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=mydb -e MYSQL_USER=docker -e MYSQL_PASSWORD=password -p 3308:3306 --name mysql mysql:5.7
  ```

## Running Spring Boot 3 Projects on Docker

### Creating image through Spring Boot -
  
  ```bash
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
