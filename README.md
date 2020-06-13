# docker-exercise
All Docker Related Exercise

# How to run MySql on Docker
  docker pull mysql
  docker run -d -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=mydb -e MYSQL_USER=docker -e MYSQL_PASSWORD=password -p 3308:3306 mysql
  
# Running Spring Boot 3 Projects on Docker

- Creating image through Spring Boot
   	   mvn spring-boot:build-image
- Running docker image -
 - Link Based Communication -
   	   docker container run -p 5000:5000 --link=charming_feynman -e RDS_HOSTNAME=charming_feynman book-store-server:0.0.1-SNAPSHOT
 - Host Based Communication -
  	    docker container run -p 5000:5000 --network=host book-store-server:0.0.1-SNAPSHOT

	- Custom Network -
		- Creating a Custom Network - 
		docker network create book-store-mysql-network
		- Running Mysql Container on that network - 
		docker run -d -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=mydb -e MYSQL_USER=docker -e MYSQL_PASSWORD=password -p 3308:3306 --name mysql1 --network=book-store-mysql-network mysql
		- Running book-store on taht network -
		docker container run -p 5000:5000 -e RDS_HOSTNAME=mysql1 --network=book-store-mysql-network --name book-store book-store-server:0.0.1-SNAPSHOT 
