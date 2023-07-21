# DOCKER IMAGES

## Docker Images

* Image ile aslinda container'i kalici hale getirmis oluyoruz.

*  An instance of an image is called a container. You have an image, which is a set of layers as you describe. If you start this image, you have a running container of this image. You can have many running containers of the same image.

* Container'lar image'lara bagli ancak image'lar containerlara bagli degil. bir image'dan birden fazla container insaa ederken tersi mumkun degil

* **Kendi Image'ini Nasil Olusturursun?**


* Dockerfile.  Bu dosyanin ismi `Dockerfile` ve uzantisi yok.
```bash
FROM Ubuntu

RUN apt-get update
RUN apt-get install python

RUN pip install flask
RUN pip install flask-mysql

COPY . /opt/source-code

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
```
* Yukaridaki kismi biraz inceleyelim.

```bash
FROM Ubuntu # Bu Dockerfile hangi isletim sisteminde calisacak, asagidakiler Ubuntu komutlari. Butun Docker dosyalari FROM ile baslamak zorundadir.

RUN apt-get update # En guncel paketlerle yoluna devam eder
RUN apt-get install python # Pyhton'u yukler

RUN pip install flask # Flask'i yukler
RUN pip install flask-mysql # Flask-mysql'i yukler

COPY . /opt/source-code # Dosyalari lokalden Docker Image'a kopyalar

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run # Updates Endpoint
```
* Dockerfile'daki komutlar image'i katman katman insaa eder. Mesela bizim ornegimizde ilk katmanda ubuntu'yu os olarak secerken son katmanda enpointleri gunceller. 

* Katmanli yapidan dolayi mesela ucuncu katmanda problem cikarsa 4 ve 5'inci katmani hic insaa etmez. 
```bash
# Image'i Dockerfile'daki gibi insaa etmek icin
docker build Dockerfile -t dosyaninkonumuveismi # RED FLAG
```
* Cikan problem duzeldikten sonra ilk 3 katmani bastan da insaa etmez, onceki bilgileri ustune koyarak devam eder. Bu noktada problemi giderdikten sonra `docker build Dockerfile -t dosyaninkonumuveismi` komutunu tekrar vermemiz gerekir.

* Tabi bu durum ben ekstra katman ekleyeyim dedigin zaman da oldukca kullanisli cunku herseyi bastan insaa etmek zorunda kalmazsin.

```bash
docker push dosyaninkonumuveismi # RED FLAG
```
------------------------------------------------------------------