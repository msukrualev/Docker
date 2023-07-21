# Docker Volumes: Lab 2

## Let's pull a Mysql Database and run it as a container

- pull the image

```bash
docker pull mysql:latest
```

- run  the container

```bash  
docker container run --detach --name mydb -e MYSQL_ROOT_PASSWORD=b107 mysql:latest
```

## Modify the database

- enter the mysql container

```bash
docker exec -it {containerID} sh
```

- enter the database client using the provided password then see the databases and tables

```bash
sh-4.4# mysql -p
```

- create a database and a table

mysql> create database test;
mysql> use test;
mysql> create table test-table;

- exit the database

mysql> exit;

## Stop and remove the container

- exit and stop the container

```bash
docker stop containerID
```

- remove the container

```bash
docker container prune
```

## Run another Mysql container and check if your database and table exist

- Run another Mysql container

```bash
docker run -d --name mydb -e MYSQL_ROOT_PASSWORD=b107 mysql:latest
```

- enter the mysql container

```bash
docker exec -it {containerID} sh
```

- enter the database client using the provided password then see the databases and tables

```bash
sh-4.4# mysql -p
```

```bash
mysql> show databases;
```

- notice that your database and files are not there, containers are stateless, they do not store your data.

- to store your data you need to create volumes.

## Run Mysql database using volumes

- Create a volume

```bash
docker volume create db-vol
```

- List volumes

```bash
docker volume ls
```

- Inspect your volume

```bash
docker inspect volumename/volumeID
```

- the output will be similar:

```json
[
    {
        "CreatedAt": "2023-04-11T09:35:43Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/db-vol/_data",
        "Name": "db-vol",
        "Options": {},
        "Scope": "local"
    }
]
```
- notice what the mountpoint indicates, this is the Linux type folder where the volume is mapped
- on macOs the real folder is ~/library/containers/com.docker.docker/Data
- on Windows it can be either \\wsl$\docker-desktop-data\version-pack-data\community\docker or C:\Users\Username\AppData\Local\Docker\wsl\data\

- now let's run the database container, this time mapping it to a local volume we just created

- note that if we did not create the volume, Docker would create it for us

```bash
docker run -d --name mydb -v db-vol:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=b107 mysql:latest
```
- here note that mapping is local folder to the folder inside the container `-v local:container`

- enter the container

```bash
docker exec -it {containerID} sh
```

- enter the database client using the provided password then see the databases and tables

```bash
sh-4.4# mysql -p
```

- create a database and a table

mysql> create database test;
mysql> use test;
mysql> create table test-table;

- exit the database

mysql> exit;


- exit, stop and remove the container

```bash
exit
docker stop containername/containerID
docker rm containername/containerID
```

- list the volumes

```bash
docker volume ls
```

- run another mysql container

```bash
docker run -d --name mydb -v db-vol:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=b107 mysql:latest
```

- enter the container and see the databases

```bash
docker exec -it {containerID} sh
mysql -p
```

```mysql
mysql> show databases;
```

- notice that your database exists, it is there, and this is because we map the container's folder to local folder, if you delete the volume locally, the data will be lost

