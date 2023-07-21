# Docker Swarm Lab

## Use Play with Docker for the Lab

- Create a cluster using the template in Docker sandbox

- create load balancer folder under root

- inside load balancer folder create Dockerfile

```Dockerfile
FROM nginx
COPY nginx.conf /etc/nginx/nginx.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

- inside load balancer folder create nginx.conf

```conf
events { worker_connections 1024; }

http {
 upstream localhost {
    server weather-app1:3000;
    server weather-app2:3000;
    server weather-app3:3000;
 }
 server {
    listen 80;
    server_name localhost;
    location / {
      proxy_pass http://localhost;
      proxy_set_header Host $host;
  }
 }
}
```

- in the main project folder create the docker-compose file

```yaml
version: '3.2'
services:
  weather-app1:
      image: mefekadocker/weather-app
      tty: true
      networks:
       - frontend
  weather-app2:
      image: mefekadocker/weather-app
      tty: true
      networks:
       - frontend
  weather-app3:
      image: mefekadocker/weather-app
      tty: true
      networks:
       - frontend

  loadbalancer:
      build: ./lb
      image: nginx
      tty: true
      ports:
       - '80:80'
      networks:
       - frontend

networks:
  frontend:
```
## Deploy the application without swarm stack

- deploy the docker-compose.yaml file

```bash
docker compose up --build
```
- using --build builds any images then creates the structure

- check the app in the browser

## Deploy the app on Docker Swarm Stack

- deploy the docker-compose.yaml file using docker stack deploy

```bash
docker stack deploy -c docker-compose.yaml weather
```
- check where the app is running

```bash
docker service ls
docker stack ls
docker service ps weather
```
- preview the app in the browser

