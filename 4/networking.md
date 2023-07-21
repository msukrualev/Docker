# Hands-on Docker-04 : Networking 


## Goals of this training

At the end of the this hands-on practice, you will;

- bind containers to specific ports

- list available networks  

- create a network  

- inspect all properties of a network  

- connect a container to a network

- explain default network bridge configuration

- configure user-defined bridge network

- ping containers within same network

- delete networks
  

## Contents

- Part 1 - Port Binding
  
- Part 2 - Default Network Bridge  

- Part 3 - User-defined Network Bridge  


## Part 1 - Port Binding

- Run a `nginx` web server, name the container as `web`, and bind the web server to host port 80. Explain `--rm` and `-p` flags and port binding.

```bash
docker run --rm -d -p 80:80 --name web nginx
# web isimli nginx imajini kullanan bir container olusturduk, --rm islemi bitince sil, -d detach modda calis, -p da port icin
```
- Kendi makinanda `localhost` (Welcome to NGINX' i gormelisin).

- Stop container `web`, should be removed automatically due to `--rm` flag.

```bash
docker stop web
```


## Part 2 - Default Network Bridge  


- List all networks available, and explain types of networks.

```bash
docker network ls

# Default networklerimiz mevcut
```

- Run two `alpine` containers with interactive shell, in detached mode, name the container as `techpro1` and `techpro2`, and add command to run alpine shell. Here, explain what the detached mode means.

```bash

# alpine imajini kullaniyoruz cunku kucuk boyutlu bir imaj
docker run -d -it --name techpro1 alpine
docker run -d -it --name techpro2 alpine
```

- Show the list of running containers on Docker machine.

```bash
docker ps
```

- Show the details of `bridge` network, and explain properties (subnet, ips) and why containers are in the default network bridge.

```bash
docker network inspect bridge # bridge network'unu inceliyoruz
# IPV6 yapilandirmasi yok
# IPV4 te 172.17.0.0/16 ya bagli, hatirlarsaniz docker 172 den veriyordu subnetleri
# Containers: Gordugunuz gibi techpro2 ve tecpro1st containerlari default olarak bridge network'une gelmis.
```


- Connect to the `techpro1` container.

```bash
# Birinci container'ima erismek istiyorum,nasil? Boyle
docker attach techpro1
```

- Ping `techpro2 `container by its IP four times to show the connection.

```bash
ping -c 3 <techpro2IPv4> # 172.17.0.3
# -c yi koymazsak sonsuza kadar calisir
```

- Try to ping `techpro2 `container by its name, should face with bad address. Explain why failed (due to default bridge configuration that does not work with container names)

```bash
ping -c 3 techpro2
# default bridge network'te direk isimle erisemiyoruz
```

- Disconnect from `techpro1` with `exit`

- Stop and delete the containers

```bash
docker stop techpro1 techpro2
docker rm techpro1 techpro2
```

## Part 3 - User-Defined Bridge Network 

- Create a bridge network `techpronet`.

```bash
# Custom bridge Network olusturalim
docker network create --driver bridge techpronet
```

- List all networks available  , and show the user-defined `techpronet`.

```bash
docker network ls
```

- Show the details of `techpronet`, and show that there is no container yet.

```bash
docker network inspect techpronet
```

- Run four `alpine` containers with interactive shell, in detached mode, name the containers as `techpro1`, `techpro2`, `techpro3` and `techpro4`. Here, 1st and 2nd containers should be in `techpronet`, 3rd container should be in default network bridge, 4th container should be in both `techpronet` and default network bridge.

```bash
# `techpronet` custom bridge network'e dahil olacak alpine imajindan `techpro1` ve `techpro2` isimli iki adet container olusturalim.
docker run -d -it --network techpronet --name techpro1 alpine
docker run -d -it --network techpronet --name techpro2 alpine

# Sonrasinda DEFAULT BRIDGE NETWORK'E BAGLI(Yukaridakiler user-defined bridge network'e bagliydi hatirlayalim)
docker run -d -it --name techpro3 alpine
docker run -d -it --name techpro4 alpine

# Son olarak techpro4'u user-defined bridge networkumuze dahil edelim
docker network connect techpronet techpro4
```

- List all running containers and show that they are up and running.

```bash
docker container ls
```

- Show the details of `techpronet`, and explore newly added containers. 


```bash
docker network inspect techpronet
# 1st, 2nd, and 4th containers should be in the list
```

- Show the details of  default network bridge, and explore newly added containers. 

```bash
docker network inspect bridge
# 3rd and 4th containers should be in the list
```

- Connect to the `techpro1` container.

```bash
docker attach techpro1
```

- Ping `techpro2` and `techpro4` container by its name to show that in user-defined network, container names can be used in networking.

```bash
ping -c 3 techpro2
ping -c 3 techpro4
```

- Try to ping `techpro3` container by its name and IP, should face with bad address because 3rd container is in different network.

```bash
ping -c 3 techpro3
ping -c 3 172.17.0.2
```

- Exit the `techpro1` container with exit.
- Connect to the `techpro4` container, since it is in both networks, it should connect all containers.

```bash
docker attach techpro4
```

- This time, try to ping `techpro3`, `techpro1`and `techpro2` containers by their names and IPs.

- Stop and remove all containers

```bash
docker stop techpro1 techpro2 techpro3 techpro4
docker rm techpro1 techpro2 techpro3 techpro4
```

- Delete `techpronet` network

```bash
docker network rm techpronet
```