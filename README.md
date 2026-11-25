1. aws

FROM maven:3.9-eclipse-temurin-17 AS build

WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

FROM tomcat:9-jdk17

COPY --from=build /app/target/*.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080

CMD ["catalina.sh", "run"]

----------------------------------------------------------------

2.docker-compose.yml

services:

  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: root123
      MYSQL_DATABASE: ecommerce
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wp123
    volumes:
      - db_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wp123
      WORDPRESS_DB_NAME: ecommerce
    volumes:
      - wp_data:/var/www/html
    depends_on:
      - db

volumes:
  db_data:
  wp_data:
-----------------------
services:
  web:
    build: .
    container_name: maven-container
    ports:
      - "3001:8080"
--------------------------------------------------------------------

3.docker-cli


FROM maven:3.9.9-eclipse-temurin-8 AS build
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

FROM tomcat:9.0-jdk8-temurin
RUN rm -rf /usr/local/tomcat/webapps/*
COPY --from=build /app/target/maven_web_java.war /usr/local/tomcat/webapps/ROOT.war
EXPOSE 8080
CMD ["catalina.sh", "run"]

4.Scripted pipeline

node {

    stage('Git Checkout') {
        git branch: 'main',
            url: 'https://github.com/sphoorthypothuganti2429-beep/javaproject.git'
    }

    stage('Clean') {
        bat 'mvn clean'
    }

    stage('Compile') {
        bat 'mvn compile'
    }

    stage('Test') {
        bat 'mvn test'
    }

    stage('Package') {
        bat 'mvn package'
    }

    stage('Archive') {
        archiveArtifacts artifacts: 'target/*.jar',
                         fingerprint: true
    }

    echo 'BUILD SUCCESSFUL!'
}

5.cli
docker pull redis
docker run --name my-redis -d redis
docker ps
docker exec -it my-redis redis-cli
SET name "Alice" 
GET name
exit
docker stop my-redis
docker start my-redis
docker rm my-redis 
docker rmi redis
6. dockerfile
o	Create a folder like C:\DockerProjects\Redis.
o	Open Git Bash and navigate to the folder:
cd /c/DockerProjects/Redis
1.	Inside the folder, create a file named Dockerfile (no extension).
2.	Add the following content:
FROM redis:latest CMD ["redis-server"]
1.	docker build -t redisnew
   2.docker run --name myredisnew -d redisnew
3. docker ps
   4. docker stop myredisnew
   5. 5.	docker login
      6.	6.	docker ps -a
         7.	7.	docker commit 0e993d2009a1 budarajumadhurika/redis1
            8.	8.	docker images
               9.	9.	docker push budarajumadhurika/redis1
                  10.	10.	docker rm 0e993d2009a1 // container id of myredisnew
                    	11.	docker rmi budarajumadhurika/redis1
                     12.	12.	docker ps -a
                         13.	13.	docker logout
                            	14.	docker pull budarajumadhurika/redis1
                             15.	15.	docker run --name myredis -d budarajumadhurika/redis1
                                 16.	16.	docker exec -it myredis redis-cli
                                    	17.	SET name "Abcdef"
                                     18.	18.	GET name
                                         19.	19.	exit
                                            	20.	docker ps -a
                                             21.	21.	docker stop myredis
                                                 22.	22.	docker rm 50a6e4a9c326
                                                    	23.	docker images
                                                     24.	24.	docker rmi budarajumadhurika/redis1
