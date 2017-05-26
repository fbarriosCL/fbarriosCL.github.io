---
layout: post
title: "Docker primeros pasos"
date:   2017-03-27 00:37:19 -0300
categories: docker
---

# Tus primeros pasos con Docker:

Docker es un ecosistema de aplicacones.

tools:

- docker machine
- docker compose
- docker engine = core de docker.

## ¿Porque docker?

millones de personas creativas.

Contenedores: Conjunto de tecnologias(namesspaces, cgroups, chroot).
namespaces: Vistas de los recursos del OS.
gruops: Limitan y miden recursos.
chroot: cambiar el root de un directory de un proceso.

Instalación de Docker.

```
$ curl -sSL get.docker.com |sh
```

```
$ docker-machine ls
NAME     ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER        ERRORS
dinghy   *        virtualbox   Running   tcp://192.168.99.100:2376           v17.03.0-ce
```

Información sobre el servidor:

```
docker info
```

```
docker version
```

Hola mundo en docker
Para ejecutar contenedores:

```
docker run hello-world
```

¿Que es un contenedor e imagenes?
un contenedor se crea a partir de una image.

Imagenes: nombre : tag(Para representar distintas versiones)

Si no se especifica toma lasted

```
docker images
```

Para descargar alguna imagen ejemplo:

```
docker pull ubuntu
```

## Conteiner 

Para ver conteiner

```
docker ps -a
```

Para ejecutar dentro del conteiner:

```
docker run ubuntu ps 
```

Para entrar al conteiner:

```
docker run -it ubuntu bash
```

```
$ docker attach conteiner
```

```
$ docker ps -a --no-trunc
```

Crear un contenedor con nombre felipe

```
$ docker run --name felipe ubuntu ls
```

Eliminar conteiner

```
$ docker rm felipe
```

Filtrar conteiner:

```
$ docker ps --filter="exited=0"
```

```
$ docker logs -f conteiner
```

Elimiar conteiner:

```
$ docker rm
```

```
$ docker rm -f
```

Elimiar todos los contenedores detenidos

Imprimimos solo el id del contenedor

```
$ docker ps -a -q
```

```
$ docker rm $(docker ps -a -q)
```

Creacion de las imagenes:

- Sistema de capas entre imagenes
- Son solo de lecturas.
- Los cambuis son realiados en la capa de escritura.


```
$ docker commit
```

Para ver los cambios que se realizaron en mi contendor:

```
$ docker diff
```

Configuración Avanzada Docker Engine.

```
$ service docker status/start/stop
```

```
$ sudo docker deamon [options] &
```

Ver los logs en upstart:

```
/var/log/upstart/docker.log
```

Varibale:

```
DOCKER_OPTS="--log-level=debug"
```

systemed

```
lib/systemed/system/docker.service
```

Configurar EnviromentFile nosotros creamos la ruta y archivo

```
EnviromentFile= /etc/default/docker
```

Creamos variable en archivo docker

OPTIONS='--log-level=debug'

```
$ sudo docker deamon --log-level=debug
```

Desventaja siempre debemos iniciar el demonio de manera manual, si se cae siempre vamos a tener que especificarlo de manera interactiva.
Siempre es bueno ocupar los paquetes de gestores de demonio.


## ¿Como conectar un demonio de forma remota?

```
/var/log/upstart/docker.log
```

Para que el servidor escuche cualquier ip disponible 0.0.0.0:

```
DOCKER_OPTS="--log-level=debug -H tcp://0.0.0.0:2375"
```

para que el cliente se pueda conectar:

```
$ docker -H tcp://0.0.0.0:2375
```

```
export DOCKER_HOST="tcp://0.0.0.0:2375"
```

## Dockerfile:

creamos imagenes con docker commit, luego sobre la imagen ejecutamos comandos, existe otra forma de crear imagenes para que no sea tan engoroso.

Docker provee dockerfile para crear imagenes en vez de ocupar docker commit, se integra de manera continua al flujo de desarrollo.

Dockerfile archivo de texto, las instrucciones mas utilizadas son FROM y RUN

Creamos archivo dockerfile tiene dos instrucciones:

```
FROM: cual va a ser la imagen base.
RUN: Podemos ejecutar varios comandos
```

Ejemplo

```
FROM ubuntu
RUN apt-get install -y curl
RUN apt-get install -y vim
```

-y para que no pida permisos.

Para construir la imagen

```
$ docker build .
```

```
$ docker run -it CONTEINER_ID bash
```

Para agregar un tag name

```
$ docker build -t NAME .
```

```
$ docker run -it NAME bash
```

## Build cache

Docker guada un snapshot de cada imagen luego de ada instruccion de build.
Docker cuando hace el build ve si esta en cache, si es que se encuentra las compara.
Para deshabilitar el cache de manera manual:

```
$ docker build --no-cache -t IMAGE
```

Historial de una imagen:
Se puede ver cada imagen, cuando fue creada y su tamaño.

```
$ docker history NAME o CONTEINER_ID
```

### CMD

Se puede usar tanto el formato Shell o Exec.

```
FROM ubuntu
CMD ping -c 10 www.google.com
```

o 

```
CMD['ping', '-c', '10', 'www.google.com']
```

### ENTRPOINT:
Define el punto de entrada con el comando que el conteiner correra cuando se crea.
Los argumentos d eruntime son enviados como parametros dinamicos.
Posee EXEC y shell.

```
FROM ubuntu
ENTRPOINT ping
```
Conteiner interactivo, defines un binario en donde le vas a poder pasar comandos.

```
$ docker run CONTEINER_ID -c 10 www.google.com
```

### Copiar archivos

Ejemplo:
- Codigo fuente.
- Archivos de configuración
- Copiar otro contenido.

```
FROM ubuntu
COPY file.txt /tmp/example
```

Matar conteiner

```
$ docker kill NAME o CONTEINER_ID
```

-P Puerto aleatorio

```
$ docker run -d -P NAME o CONTEINER_ID
```

### Add

Mismo formato instruccion COPY y ampos opetan de manera similar.
ADD permite descomprimir archivos.
ADD permite obtener archivos de una URL.

ambas utilizan checksum de los archivos añadidos para calcular la cache.

```
FROM ubuntu
RUN apt-get update && apt-get install -y jp2a
ADD: http://cdn.meme.am/instances/66627195.jpg /tmp/img.jpg
ENV TERM xterm-256color
CMD jp2a --size=60x40 /tmp/img.jpg
```

```
$ docker run NAME o CONTEINER_ID
```

Otras instrucciones

MAINTEINER agrega metdata al dockerfile sobre el dueño de la imagen.

ENV variables de entorno al contenedor.

LABEL para agregar etiquetas al contenedor.

Para subir a dockerhub

```
$ docker tag NAME o CONTEINER_ID userdockerhub/name
```

```
$ docker push username/name
```

Borrar imagenes

```
docker rmi
```

### Volumenes

Un directorio designado en el contendor el cual nos permite persistir datos independiente del ciclo de vida del contenedor.

Pueden estar mapeados a un directorio comun, se puede compartir entre conteiner.

La instruccion RUN del dockerfile no modifican la informacion de los volumenes.


El comando ```docker volume```contiene subcomando para la gestion de volumenes en docker:

Para crear un volumen
```
$ docker volume create
$ docker volume ls
$ docker volume inspect
$ docker volume rm
```

Para montar un volumen se utiliza -v en el comando ```docker run```

Se pueden montar directorios del host con el formato ```-v [host path]:[conteiner path]:[ro|rw]```


return id de los volumenes

```
$ docker volume ls -q
```

Borrar los volumenes:

```
$ docker volume rm(docker volume ls -q)
```

Para crear un volumen con nombre prueba

```
$ docker volume create --name prueba
```

Para especificar nuestro volumen a un conteiner, en este ejemplo ubuntu:

```
$ docker run -ti -v prueba prueba:/prueba ubuntu bash
```

Para inspeccionar un volumen:

```
$ docker volume inspect prueba
```

Para eliminar un volumen

```
$ docker volume rm NAME o CONTEINER_ID
```

Crear volumenes anonimos:

```
$ docker run -ti -v /home/users/example:/example ubuntu bash
```


## Docker compose

Compose es una herramienta para crear y administrar aplicaciones multi-contenedor.

- Arquitectura microservicios.
- Muy tedioso ejecutar docker RUN

docker-comopse.yml Define los servicios que componene la aplicacion.

Cada servicio contiene las intrucciones para construir y ejecutar el contenedor.

```
web:                              < SERVICIO
  build: .                        < RUTA BUILD
  command: python example.py      < COMANDO INCIO
  links:                          < LINK A CONTENEDOR
    - redis                        
  redis:                          
    image: redis                  < IMAGEN
```

https://hub.docker.com/r/dockercloud/haproxy/

