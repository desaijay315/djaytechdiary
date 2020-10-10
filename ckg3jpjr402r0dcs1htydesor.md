## Discovering Docker Data Volumes

If you have followed through the last article on Docker, we have launched docker container from Redis image. Based on the learnings, you would have seen that once the docker container is removed, that data inside the container also gets destroyed, this is due to the fact that Docker containers are Ephemeral. 

In this article, we will learn why we require docker volumes, and how we can persist data even if the container is stopped and removed. We can surely achieve this by knowing the fundamental fact that we would separate our container lifecycle from the data present inside the container. This means that once the docker container is removed, our data will still be persisted the next time we launch the container with the same volume. Let us see how we can apply the same in regards to the MYSQL database docker image.

This article assumes you have a basic understanding of docker images and containers.

- To learn more about docker, here you go -


%[https://djaytechdiary.com/what-is-docker-and-why-it-is-so-popular]


### Docker Volumes

Volumes and bind mounts are the two options within the Docker to store files in the host machine so that the files are persisted even after the container is stopped & removed. We would be using Volumes, as it is the best way to persist our data in docker

Let's understand what purpose our volume will serve. Let assume we are using Docker image of MYSQL to store our application data. 

We would definitely want our data to be persisted across the containers even if the containers are stopped or removed.

### MYSQL docker container


- Create and run the container

```
docker run --name mysqldb -p 3306:3306 -e MYSQL_ROOT_PASSWORD=mypassword -d mysql:latest

``` 

- Execute an interactive bash shell on the container


```
docker exec -it mysqldb bash

``` 

- Connect to this database

```
mysql -h127.0.0.1 -pmypassword -uroot
``` 

- Create database 


```
create database dockerexample;

``` 

- Use database


```
use dockerexample;

``` 

- Create a users table 

```
CREATE TABLE `users` (
     `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
     `userName` varchar(32) DEFAULT NULL,
     `password` varchar(32) DEFAULT NULL,
     PRIMARY KEY (`id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8;

``` 

 - Insert a row

``` 
insert into `users` (userName,password) VALUE ('jaydesai', 'thisismypassword');
insert into `users` (userName,password) VALUE ('johndoe', 'thisisjohnspassword');
``` 

- This shows two users in our table

```
mysql> select * from users;
+----+----------+---------------------+
| id | userName | password            |
+----+----------+---------------------+
|  1 | jaydesai | thisismypassword    |
|  2 | johndoe  | thisisjohnspassword |
+----+----------+---------------------+
2 rows in set (0.00 sec)

``` 

- Now, stop the container and remove it.


```
docker stop mysqldb && docker rm mysqldb

``` 

- To verify whether the container has been removed

```
docker container ls -a
``` 


- Now, let's go again and re-run the above commands to create MySQL container and follow all the above steps. 
- You will be able to see that the database you created has been destroyed on stopping and removing the container.


```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.02 sec)

``` 

- This is due to the fact that the data gets destroyed whenever we stop or remove from the container.

### MYSQL docker container with docker volume

- Create a volume(one-time creation)


```
docker volume create mysql-data
``` 

- Create & run the container with the volume

```
docker run --name mysqldb -p 3306:3306 -e MYSQL_ROOT_PASSWORD=mypassword -v mysql-data:/var/lib/mysql -d mysql

``` 

- Verify the volume is mounted. You will the source with the mysql-data


```
...
"Mounts": [
            {
                "Type": "volume",
                "Name": "mysql-data",
                "Source": "/var/lib/docker/volumes/mysql-data/_data",
                "Destination": "/var/lib/mysql",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            }
        ],
...
``` 

- Now again Execute an interactive bash shell on the container
- Perform the above step to create the database, users table, and insert the user data
- Next, stop & remove the container. 


> 
What do you think will happen now? Will the database be persisted or dropped?? Let's verify the same

- Create and Run the container

```
docker run --name mysqldb -p 3306:3306 -e MYSQL_ROOT_PASSWORD=mypassword -v mysql-data:/var/lib/mysql -d mysql
``` 

- Now, verify if the database is still present. 


```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| dockerexample      |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.28 sec)
``` 

- Also, the user's table with the data is present

```
mysql> select * from users;
+----+----------+---------------------+
| id | userName | password            |
+----+----------+---------------------+
|  1 | jaydesai | thisismypassword    |
|  2 | johndoe  | thisisjohnspassword |
+----+----------+---------------------+
2 rows in set (0.00 sec)

``` 

Similarly, you can create the volumes for different database. I have created a  [GitHub](https://github.com/desaijay315/docker-volumes/wiki)  wiki showing examples on MongoDB and PostgreSQL Database:


-  [MongoDB on Docker](https://github.com/desaijay315/docker-volumes/wiki/MongoDB-Docker-Volume) 
-  [PostgreSQL on Docker](https://github.com/desaijay315/docker-volumes/wiki/PostGreSQL-Database-Docker-Volume) 



### Conclusion

In this article, we have learned what is docker volumes, why to create docker volumes and how we can persist data in the docker volumes with different databases.

If you liked it please leave some claps to show your support. Also, leave your responses below and reach out to me if you face any issues.