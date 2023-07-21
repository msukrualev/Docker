# Docker Compose Lab: Replicas

This lab defines basics of a Docker Compose file.

>> !!!
**Make sure you have yaml extensions installed in your vscode environment.**
>> !!!


## Create a folder and a docker compose file

- create a project folder and cd into it

```bash
mkdir compose2
cd compose2
```

- create a docker-compose.yaml file

```yaml
version: "3"
services:
  web:
    image: nginx:alpine
    deploy:
      replicas: 2
    ports:
     - 5000-5001:80
```

- still inside the project folder where there is the docker-compose.yaml file is run docker compose command

```bash
docker compose up
```
- running command with -d flag runs in the background

- you can now see the running service on the browser `localhost`, switching ports 5000-5001

- check the running containers

```bash
docker ps
```

- tear down the service

```bash
docker compose down
```
