# Build a simple Docker Image using Dockerfile

## Manual Steps


- pull an ubuntu image

```bash
docker pull ubuntu
```


- run the container interactively

```bash
docker run -it ubuntu
```


- update packages in the container

```bash
apt-get update -y
```

- install figlet app

```bash
apt-get install figlet
```

- run the app

```bash
figlet Merhaba B107!
```


## Do with Dockerfile

- create the Dockerfile with nano or your favorite editor

```Dockerfile
FROM ubuntu
RUN apt-get update
RUN apt-get install figlet
ENTRYPOINT figlet Merhaba B107
# CMD can also be used here
```
- build the image

```bash
docker build -t fig-app .
```

- view the images

```bash
docker images
```

- run the image as a container

```bash
docker run fig-app
```
 __  __           _           _             ____  _  ___ _____ 
|  \/  | ___ _ __| |__   __ _| |__   __ _  | __ )/ |/ _ \___  |
| |\/| |/ _ \ '__| '_ \ / _` | '_ \ / _` | |  _ \| | | | | / / 
| |  | |  __/ |  | | | | (_| | |_) | (_| | | |_) | | |_| |/ /  
|_|  |_|\___|_|  |_| |_|\__,_|_.__/ \__,_| |____/|_|\___//_/   


## Pass arguments to container at runtime using ENTRYPOINT exec mode

- modify the Dockerfile

```Dockerfile
FROM ubuntu
RUN apt-get update
RUN apt-get install figlet
ENTRYPOINT ["figlet", "Merhaba"]
```

- build the image

```bash
docker build -t fig-app2 .
```

- view the images

```bash
docker images
```

- run the image as a container, this time passing an argument

```bash
docker run fig-app2 Dockerseverler!
```

__  __           _           _           
|  \/  | ___ _ __| |__   __ _| |__   __ _ 
| |\/| |/ _ \ '__| '_ \ / _` | '_ \ / _` |
| |  | |  __/ |  | | | | (_| | |_) | (_| |
|_|  |_|\___|_|  |_| |_|\__,_|_.__/ \__,_|
                                          
 ____             _                                     _           
|  _ \  ___   ___| | _____ _ __ ___  _____   _____ _ __| | ___ _ __ 
| | | |/ _ \ / __| |/ / _ \ '__/ __|/ _ \ \ / / _ \ '__| |/ _ \ '__|
| |_| | (_) | (__|   <  __/ |  \__ \  __/\ V /  __/ |  | |  __/ |   
|____/ \___/ \___|_|\_\___|_|  |___/\___| \_/ \___|_|  |_|\___|_|   

