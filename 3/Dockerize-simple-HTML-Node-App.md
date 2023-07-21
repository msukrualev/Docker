# Dockerize HTML page and a Node App

Docker Hub is a registry, collection of repos
On Docker Hub you can have public or private repositories for your applications
Repository is having versions of an application, collection of related images

## Convert your index.html simple web page to a Docker Image

- create index.html file

```html
<h1>Welcome to our web page</h1>

<h2>DevOps Batch 107</h2>
```

- create a Dockerfile

```Dockerfile
FROM nginx:1.22-alpine
COPY . /usr/share/nginx/html

```
- build the image

```bash
docker build -t mysite .
```
- run the image as container

```bash
docker run -d -p 80:80 mysite:latest
```

- view the page on a browser on http://localhost


## Create a simple Node app

- create a node-app folder
- create a src folder
- inside src folder create server.js file

```js
const express = require('express');
const app = express();

app.get('/', (req,res) => {
	res.send("Welcome to B107 node app!");
});

app.listen(3000, function () {
	console.log("app listening on port 3000");
});
```
- create package.json file in project root

```json
{
	"name":"my-app",
	"version":"1.0",
	"dependencies": {
		"express": "4.18.2"
	}
}
```

- create Dockerfile

```Dockerfile
FROM node:19-alpine
WORKDIR /app
COPY package.json .
COPY src .
# run linux shell command
RUN npm install 
# instruction to be executed when container starts
CMD ["node","server.js"]
```

- then build the image

```bash
docker build -t node-app .
```

- now run the image as container

```bash
docker run -d -p 80:3000 node-app:1.0
```

- view through a browser using http://localhost


