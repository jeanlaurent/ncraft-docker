# Workshop Docker Deep Dive (105)

## Description

In this workshop, you will learn everything you need to know to get yourself up and running and comfortable with Docker. Come as you are with your laptop, isn’t necessary to have knowledge about Docker with Windows Containers or with docker on Linux. The aim is to discover with examples what’s going on about containers and how to leverage it in your developer life.  

## Introduction 5 minutes
 * Jean-Laurent - Docker
 * Bruno - 42 SKILLZ
 
Slides

 1. Docker the company
 2. Docker in France
 3. R&D
 4. Container vs VM
 5. Docker vocabulary
 6. WifiCode

## Introduction & Prerequisites (15)

### Docker for Windows 10 >= 10586
 * Install Docker for Windows
  * visit https://docs.docker.com/docker-for-windows/install/
  * Choose the edge flavor
  * Install it
  * Switch to windows containers
  * run `docker pull microsoft/iis:nanoserver`
 or
 * docker load -i d:\tar\microsoft-dotnet-nanoserver.tar
 * docker load -i d:\tar\microsoft-iis-nanoserver.tar


# What’s a container? (10)
```
> mkdir workshop

> cd workshop

Switch to Linux containers

> docker version

> docker info

> docker ps

> docker images

> docker run hello-world

> docker images

> docker ps -a

> docker rm <contain_id>
```
#### Switch to Windows Containers (15)
```
> docker version

> docker info

> docker ps

> docker images

    go to https://hub.docker.com/

    Search "nanoserver"

> docker run -it --rm microsoft/nanoserver powershell

> ps

> ls
```

[Launch another powershell side by side with the previous]
```
> docker run -it --rm microsoft/nanoserver powershell

> ps

> ls

> rm Licences.txt

> exit

> docker images

> docker run -it --rm microsoft/dotnet:nanoserver dotnet --version
```

# Inside a container (15)
```
> docker run -d --rm -p 81:80 microsoft/iis:nanoserver

> docker ps

> docker inspect <containerId>

[peek the ip address]
[on the host, browse the ip]

> docker exec -it <containerId> powershell

> ls

> cd C:\inetpub\wwwroot

> ls

> echo "Rocks Docker for Windows" > index.htm

[on the host, refresh the browser]

docker stop <containerId>

```
# Inside an image (15)

`docker save microsoft/iis:nanoserver -o iisnano.tar`

#### Switch to Linux containers
```
> docker ps

> docker images

> docker pull alpine

> docker images
```
## mount a volune

```
> md c:/app

> echo "Hello" > c:/app/hello.txt

> docker run -v c:/app:c:/ncraft microsoft/nanoserver powershell -c Get-Content c:/ncraft/hello.txt

> docker run -it -v c:/app:c:/ncraft microsoft/nanoserver powershell

> ls

> cd ncraft

> echo "World" >> hello.txt

> exit

> Get-Content c:\app\hello.txt
```

## Shared Drives
```
In your working directory

> mkdir extract
```
#### Switch to Linux containers
```
> docker run --rm -v c:/users:/data alpine ls /data

> docker run -it --rm -v c:/users/{user}/workshop:/data alpine sh

> tar -xf data/iisnano.tar -C data/extract

    Explore on windows C:\Users\{user}\data\extract
```
### Describe app and the structure

> https://github.com/warpdesign/html5-solitaire-js

> git clone https://github.com/warpdesign/html5-solitaire-js.git

> move html5-solitaire-js.git to c:/app

### Setup the volume if need
```
> docker run --rm -it -v c:/app:/usr/share/nginx/html -p 8080:80 nginx
```
#### Switch to Windows Containers
```
> docker run --rm -it -v c:/app:/usr/share/nginx/html -p 8080:80 microsoft/iis
```
# Copy concept (15)

#### Switch to Linux containers
```
> docker run -d -p 80:80 nginx

> docker ps

> docker cp .\app\. <container_id>:/usr/share/nginx/html

> docker commit <container_id> nginx:solitaire

> docker history nginx:solitaire
```
## Dockerfile for windows container (15)

#### Switch to Windows Containers

Edit Dockerfile
```
FROM microsoft/iis:nanoserver

COPY app c:/inetpub/wwwroot
```
```
> docker build -t iis:solitaire .

> docker images

> docker run --rm -d -p 81:80 iis:solitaire

> docker ps

> docker inspect <containerID>

[peek the ip address]
[on the host, browse the ip]

```
# Composing applications (15)

https://github.com/dgageot/dockercon16
