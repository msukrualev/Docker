# Hands-on Docker-Volumes-Lab3 : Bind Mount

## Learning Outcomes

- Use a local folder as a mount point for Bind Mount
- Start a container as an nginx web server, mapping the local folder to nginx folder inside container


## Stage 1 

- create a folder on your local machine

- clone the website repo inside the folder

`git clone https://github.com/talfik2/javascript-projects`

- change directory into reviews folder where the index.html and other files are

## Stage 2

- Run the `nginx` container in detached mod, name it as `web_server`, map the local folder to the nginx folder inside the container

```bash
docker run -d --name web_server --rm -p 8080:80 --mount type=bind,source=/Users/mefeka/website/javascript-projects/reviews,target=/usr/share/nginx/html nginx:alpine
# -d yani detached modda calis
# -p 8080:80 yani portlari birbirine maruz birak?
# --rm yani container islemini tamamladiktan sonra sil
# nginx imaji kullanacagiz

Burada <http://localhost:8080/> girip web sitesini gormen lazim. Saniyeler icerisinde bir web sayfasini ayaga kaldirdik, web sitesi içeriği ise yerel makinamız, host içerisindei iste dockerin guzelligi bu..
```

## Stage -3 


- Remove all containers and images

```bash
docker rm -f web_server
```
