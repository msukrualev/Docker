## Create a simple Dockerized Javascript app
# Prerequisites
- To be able to Dockerize a Node Javascript app you need to make sure you
 have node runtime locally,
 have app file,
 can run the app, 
  `node app.js`

- inside a local folder
- create a file named app.js
```javascript
console.log("Merhaba Batch107 Dockerseverler");
```

- if you do not have NodeJs installed on your computer you can not run this

- run a node.js container

```bash
docker run -it node:alpine sh
```

- open another terminal keeping the first one and find the container ID

`docker ps`

- browse to the folder containing the Javascript file and then copy the `app.js` file into the container

```bash
docker cp app.js containerID:/
```

- in the terminal that runs the container see the copied app.js, then run the node command to run the simple app.

```bash
node app.js
```


# A Dockerfile contains instructions to pack and run the application

- Create the Dockerfile

```Dockerfile
# use node.js image which runs on alpine linux distro, which is a lightweight distro
FROM node:alpine 

# copy the contents of the current folder to the app directory inside the image we create
COPY . /app 

# change directory to app directory
WORKDIR /app

# run the application that we copied inside the image
CMD node app.js
```

# Build the image using the Dockerfile

- As the Dockerfile contains instructions to create the image, we can use it to create our image.

```bash
docker build -t nodejs-app .
```

# Run the image as a container

```bash
docker run nodejs-app
```

- output:

Merhaba Batch107 Dockerseverler



