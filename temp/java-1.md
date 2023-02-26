# final Notes

# dockerize 'Java Login App' in 3 tier container
- taken from video 3rd


 
## launch one ec2-instance


## install docker in amazon linux
```
suso su -
yum update -y
yum install docker -y
systemctl start docker
systemctl enable docker
systemctl status docker

# create docker network
docker network create webnet		# webnet is here network_name
```


### go to docker hub site 
- search mysql>> click on mysql image>> overview>>





## ============= DB_Layer ======================


### create Dockerfile & init.sql for mysql image
```
pwd			#/root
mkdir mysql
cd mysql

# create init.sql file inside 'scripts' folder
mkdir scripts
cd scripts
vim init.sql

# see sql queries to create database & table to store data from our application
cat init.sql
# Create UserDB database
CREATE DATABASE IF NOT EXISTS UserDB;

# Use UserDB database
USE UserDB;

# Create Employee table
CREATE TABLE IF NOT EXISTS Employee (
  id INT UNSIGNED AUTO_INCREMENT NOT NULL,
  first_name VARCHAR(250),
  last_name VARCHAR(250),
  email VARCHAR(250),
  username VARCHAR(250),
  password VARCHAR(250),
  regdate TIMESTAMP,
  PRIMARY KEY (id)
);


cd ../
pwd 		#/root/mysql


# create Dockerfile
vim Dockerfile

# see Dockerfile
cat Dockerfile
FROM mysql:5.7
ENV MYSQL_ROOT_PASSWORD admin123
ENV MYSQL_DATABASE UserDB
ENV MYSQL_USER admin
ENV MYSQL_PASSWORD admin123
COPY ./scripts /docker-entrypoint-initdb.d/
```


### build image (mysql_image) & create container(mysql_cont)
```
pwd 	#/root/mysql

# build image
docker build -t mysql_image .

# see images
docker images

# create container(mysql_cont)
docker run --name mysql_cont -d -p 3306:3306 --network webnet mysql_image

# see container
docker ps
```



### go to your github repo 'java-project' & clone into your local pc & update 'application.properties' file with mysql_credentials
```
mkdir local-pc-folder
cd local-pc-folder
git clone git@github.com:Hamid-R1/java-project.git	  #use ssh url, bcuz my pc to github integration has setup as ssh-key,#
cd java-project/
cd src/main/resources/
ls -l
vim application.properties		#update 'application.properties' file with mysql_credentials


# see updated files below
cat application.properties
spring.mvc.view.prefix=/pages/
spring.mvc.view.suffix=.jsp

# Connection url for the database "netgloo_blog"
spring.datasource.url = jdbc:mysql://mysql_cont:3306/UserDB

# Username and password
spring.datasource.username = admin
spring.datasource.password = admin123


## go to 'java-project' folder push to your repo
cd ../../../
pwd			#/c/Users/OS_X/Desktop/local-pc-folder/java-project

# do git add, git commit & git push
git add .
git commit -m "updated this file application.properties"
git push -u origin main
$ git push -u origin main		#push completed,#
```

## ============= upto here DB_Layer is completed ======================







## ============= Application_Layer ======================


### go to root home directory & do this
```
pwd      #/root
mkdir tomcat
cd tomcat/


# create Dockerfile for tomcat application
vim Dockerfile

# see Dockerfile
cat Dockerfile
FROM amazonlinux AS build
ENV JAVA_HOME /opt/jdk11
ENV PATH $PATH:/opt/jdk11/bin
ENV M2_HOME /opt/maven
RUN export PATH
RUN export JAVA_HOME
RUN yum install wget -y && \
    yum install gzip -y && \
    yum install tar -y && \
    yum install git -y && \
	yum install telnet -y
RUN wget https://download.java.net/openjdk/jdk11/ri/openjdk-11+28_linux-x64_bin.tar.gz && \
    tar zxf openjdk-11+28_linux-x64_bin.tar.gz && \
    mv jdk-11 /opt/jdk11

RUN wget https://archive.apache.org/dist/maven/maven-3/3.8.6/binaries/apache-maven-3.8.6-bin.tar.gz && \
    tar zxf apache-maven-3.8.6-bin.tar.gz && \
    mv apache-maven-3.8.6 /opt/maven

RUN git clone https://github.com/Hamid-R1/java-project.git /opt/app
WORKDIR /opt/app
RUN /opt/maven/bin/mvn package


# 2nd images for tomcat app
FROM amazonlinux
ENV JAVA_HOME /opt/jdk11
ENV PATH $PATH:/opt/jdk11/bin
RUN yum install wget -y && \
    yum install gzip -y && \
    yum install tar -y && \
	yum install telnet -y
RUN wget https://download.java.net/openjdk/jdk11/ri/openjdk-11+28_linux-x64_bin.tar.gz && \
    tar zxf openjdk-11+28_linux-x64_bin.tar.gz && \
    mv jdk-11 /opt/jdk11
RUN wget https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.82/bin/apache-tomcat-8.5.82.tar.gz && \
    tar zxf apache-tomcat-8.5.82.tar.gz && \
    mv apache-tomcat-8.5.82 /opt/tomcat
COPY --from=build /opt/app/target/dptweb-1.0.war /opt/tomcat/webapps/
CMD  ["/opt/tomcat/bin/catalina.sh", "run"]
```




### build image from Dockerfile & create container
```
# build Dockerfile
pwd 		#/root/app
docker build -t tomcat_image .

# see images
docker images

# create container from tomcat_image
docker run --name tomcat_cont -d -p 8080:8080 --network webnet tomcat_image

# see container
docker ps

# go inside container tomcat_cont
docker exec -it <container_id or container_name> /bin/bash


# see the conectivity from mysql_cont(mysql_container) to this tomcat_cont(container)
bash-4.2# telnet <container_name/mysql_cont/rds_endpoint> 3306		#u get output like connected.... 

exit
```

## ============= upto here Application_Layer is completed ======================








## ============= Web_server_Layer ======================



### go to root home directory & do this
```
pwd      #/root
mkdir nginx
cd nginx

# make nginx.conf
vim nginx.conf


# see content of nginx.conf
```
# see content
cat nginx.conf
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 4096;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80;
        listen       [::]:80;
        server_name  _;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;
        location / {
           proxy_pass http://tomcat_cont:8080/dptweb-1.0/;
        }


        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
    }

# Settings for a TLS enabled server.
#
#    server {
#        listen       443 ssl http2;
#        listen       [::]:443 ssl http2;
#        server_name  _;
#        root         /usr/share/nginx/html;
#
#        ssl_certificate "/etc/pki/nginx/server.crt";
#        ssl_certificate_key "/etc/pki/nginx/private/server.key";
#        ssl_session_cache shared:SSL:1m;
#        ssl_session_timeout  10m;
#        ssl_ciphers PROFILE=SYSTEM;
#        ssl_prefer_server_ciphers on;
#
#        # Load configuration files for the default server block.
#        include /etc/nginx/default.d/*.conf;
#
#        error_page 404 /404.html;
#            location = /40x.html {
#        }
#
#        error_page 500 502 503 504 /50x.html;
#            location = /50x.html {
#        }
#    }

}

```


### create Dockerfile for tomcat_image
```
# create Dockerfile
vim Dockerfile

# see Dockerfile content
cat Dockerfile
FROM amazonlinux
RUN amazon-linux-extras install nginx1 -y
RUN yum install telnet -y
COPY ./nginx.conf /etc/nginx/nginx.conf
CMD ["nginx", "-g", "daemon off;"]
```



### build image from Dockerfile & create container
```
pwd 		#/root/nginx
docker build -t nginx_image .

# see images
docker images

# create container from 'nginx_image'
docker run --name nginx_cont -d -p 80:80 --network webnet nginx_image

# see container
docker ps

# go inside nginx_cont container
docker exec -it <container_id or container_name> /bin/bash

# see connectivity between this 'nginx_cont' to 'tomcat_cont' container
bash-4.2# telnet tomcat_cont 8080			# telnet container_name port

exit
```



### Next
- copy public ip of instance & paste to new tab>> here shown our app>>do this:
- click on login-here>>click register here>> fill details & submit>>
- click on login here>> fill  login credentials & login successful>>done



### Next
- go to mysql_cont(container) 
- go to mysql engine/client >> check database, table & confirm, see:
```
# see container
docker ps

# go inside mysql_container(mysql_cont)
docker exec -it mysql_cont /bin/bash

# go inside database client to see databases & tables
bash-4.2# mysql -u root -p UserDB		#type password when promted

mysql>


## see database & tables
```
# How to see list of Databases
SHOW DATABASES;


# How to list Tables
USE UserDB;

SHOW TABLES;


# List Table data
SELECT * FROM Employee;

```

## ============= upto here Web_server_Layer is completed ======================








