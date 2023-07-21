# Docker Compose flask-app

## Prerequisites

You need to have Docker Engine and Docker Compose on your machine.

## Step 1: Define the application dependencies

- Create a directory for the project:

```bash
mkdir compose
cd compose
```

- Create a file called app.py in your project directory and paste the following code in:

```python
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return 'Merhaba! Sayfayı {} defa tıkladınız.\n'.format(count)
```

- Create another file called requirements.txt in your project directory and paste the following code in:

```requirements.txt
flask
redis
```

## Step 2: Create a Dockerfile

The Dockerfile is used to build a Docker image. The image contains all the dependencies the Python application requires, including Python itself.

In your project directory, create a file named Dockerfile and paste the following code in:

```Dockerfile
# syntax=docker/dockerfile:1
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
EXPOSE 5000
COPY . .
CMD ["flask", "run"]
```

This tells Docker to:

- Build an image starting with the Python 3.7 image.
- Set the working directory to /code.
- Set environment variables used by the flask command.
- Install gcc and other dependencies
- Copy requirements.txt and install the Python dependencies.
- Add metadata to the image to describe that the container is listening on port 5000
- Copy the current directory . in the project to the workdir . in the image.
- Set the default command for the container to flask run.


## Step 3: Define services in a Compose file

Create a file called docker-compose.yml in your project directory and paste the following:

```yaml
version: "3.9"
services:
  web:
    build: .
    ports:
      - "8000:5000"
  redis:
    image: "redis:alpine"
```

This Compose file defines two services: web and redis.

The web service uses an image that’s built from the Dockerfile in the current directory. It then binds the container and the host machine to the exposed port, 8000. This example service uses the default port for the Flask web server, 5000.

The redis service uses a public Redis image pulled from the Docker Hub registry.

## Step 4: Build and run your app with Compose

- From your project directory, start up your application by running docker compose up.

```bash
docker compose up
```
Compose pulls a Redis image, builds an image for your code, and starts the services you defined. In this case, the code is statically copied into the image at build time.

- Enter http://localhost:8000/ in a browser to see the application running.

If this doesn’t resolve, you can also try http://127.0.0.1:8000.

You should see a message in your browser saying:
Hello World! I have been seen 1 times.

## Step 8: Experiment with some other commands

- If you want to run your services in the background, you can pass the -d flag (for “detached” mode) to docker compose up and use docker compose ps to see what is currently running:

```bash
docker compose up -d
```
```bash
docker compose ps
```
- The docker compose run command allows you to run one-off commands for your services. For example, to see what environment variables are available inside the web service:

```bash
docker compose run web env
```

- see what type of is inside redis service/container
```bash
docker compose run redis cat /etc/os-release
```

## Step 9: Tear down the services, removing all

```bash
docker compose down
```


