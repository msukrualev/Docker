# Docke Compose with Mongo and Mongo-Express

## Pull Mongo and Mongo-express, Create Network,Run Containers with required Environment variables

- pull mongo:4

- pull mongo-express:latest

- create a network for the database containers

```bash
docker network create --subnet 180.1.0.0/24 --gateway 180.1.0.1 mongo-net
```

- inspect the network to see the CIDR and gateway

```bash
docker inspect network mongo-net
```

- run mongo containers on the mongo-net network

```bash
# here with -e flag we define credentials to login to database
docker run -p 27017:27017 \
 -d -e MONGO_INITDB_ROOT_USERNAME=admin \
 -e MONGO_INITDB_ROOT_PASSWORD=password \
 --name mongodb --net mongo-net mongo:4
```

- run mongo-express to be able to connect to mongo

```bash
# here with -e flag we define credentials to login to database
docker run -d -p 8081:8081 \
-e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
-e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
-e ME_CONFIG_MONGODB_SERVER=mongodb \
--net mongo-net \
--name mongo-ex \
mongo-express
```

- now, use mongo-express to connect to mongo server, view databases, create databases or collections..

localhost:8081

- remove containers and network

```bash
docker stop mongodb mongo-ex
docker rm mongodb mongo-ex
docker network rm mongo-net
```


## Now create the same structure on the previous step this time using Docker Compose

```yaml
version: "3"
services:

  mongodb:
    image: mongo:4
    networks:
     - mongo-net
    ports:
     - 27017:27017
    environment:
     MONGO_INITDB_ROOT_USERNAME: admin
     MONGO_INITDB_ROOT_PASSWORD: password
  mongo-ex:
    depends_on:
     - mongodb
    image: mongo-express
    networks:
     - mongo-net
    ports:
     - 8081:8081
    restart: always
    environment:
     ME_CONFIG_MONGODB_ADMINUSERNAME: admin
     ME_CONFIG_MONGODB_ADMINPASSWORD: password
     ME_CONFIG_MONGODB_SERVER: mongodb
networks:
  mongo-net:
   driver: bridge
   ipam:
    config:
     - subnet: 180.1.0.0/24
       gateway: 180.1.0.1
``






