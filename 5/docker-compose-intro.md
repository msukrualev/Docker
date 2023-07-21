# Introductory Docker Compose Lab

This lab defines basics of a Docker Compose file.

>> !!!
**Make sure you have yaml extensions installed in your vscode environment.**
>> !!!

## Imperative and Declarative Forms

```bash
docker run --name web -p 80:80 nginx
```

```yaml
version: "3"
services:
  web:
   image: nginx
   ports:
    - 80:80
```
- notice that the key objects and parameters match. web is the container/service name, -p is ports and nginx is still the image name


## Create a folder and a docker compose file

- create a project folder and cd into it

```bash
mkdir compose1
cd compose1
```

- create a docker-compose.yaml file

```yaml
version: "3"
services:
  web:
   image: nginx
   ports:
    - 80:80
```

- still inside the project folder where there is the docker-compose.yaml file is run docker compose command

```bash
docker compose up
```
- running command with -d flag runs in the background

- you can now see the running service on the browser `localhost`

- check the running containers

```bash
docker ps
```

- check the network list. what did you notice? docker compose creates its own network.

```bash
docker network ls
```


- tear down the service (remember AWS cloudformation stacks)

```bash
docker compose down
```

- notice that created resoruces/objects are removed. check if the container exists.

```bash
docker ps -a 
```

## Modify docker compose file

- Now modify docker-compose.yaml file. Add another service.

- Note that the identical resource names (services) should be on the same vertical line

```yaml
version: "3"
services:
  web:
   image: nginx
   ports:
    - 80:80
  web2:
   image: httpd
   ports:
    - 8080:80
```

- Now, bring up the services again

```bash
docker compose up -d
```

- Check the services on the browser using the corresponding ports.

- you can tear down the service 

```bash
docker compose down
```

## Modify docker compose file for networks

```yaml
version: "3"
services:
  web:
   image: nginx
   networks:
    - web-net
   ports:
    - 80:80
  web2:
   image: httpd
   networks:
    - web-net
   ports:
    - 8080:80
networks:
  web-net:
```

- this way, docker compose creates user-defined network

- bring up the services

```bash
docker compose up -d
```

- check the network names

```bash
docker network ls
```

- check the services/containers for their networks

```bash
docker inspect web2
```

## Check network connectivity

- Connect to one of the services/containers

```bash
docker exec -it containerID
```

- ping/curl the other service/container

```bash
ping/curl containerIP
```

- use its name this time

```bash
ping/curl containerName
```

- tear down the services

```bash
docker compose down
```

***Notice that with user defined network and docker compose, your services can communicate with each other using their names**



## Modify docker compose file for volumes


- Now, again let's modify docker-compose file.

- If you are on Docker Desktop, go to settings and share your project folder


`share a local folder as a volume`
```yaml
version: "3"
services:
  web:
   image: nginx
   volumes:
    - /data/web:/usr/share/nginx/html
   ports:
    - 80:80  
```
- save the docker-compose.yaml file

- bring up the services

```bash
docker compose up -d
```

- see the result in browser

- go to the shared folder

- create a simple web page and save as index.html

```html
<!DOCTYPE html>
<html>
<body style="background-color:powderblue;">
<h1 style="font-family:verdana;">Merhaba B107</h1>
<p style="font-family:courier;">Docker bir harika! oyle degil mi?</p>
</body>
</html>
```

- now refresh the page on the browser to view the new web page content

- With this second way you create a named folder using docker-compose, when you tear down the structure, it is also removed. 

`share a named/created folder`
```yaml
version: "3"
services:
  web:
   image: nginx
   volumes:
    - data:/usr/share/nginx/html
   ports:
    - 80:80
volumes:
  data:  
```

- Tear down the services, use -v flag to tear down the volumes too.

```bash
docker compose down -v
```
