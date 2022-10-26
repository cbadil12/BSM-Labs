# CLASE 0: COMANDOS ÚTILES PARA LA TERMINAL
 - `ls` Lista el contenido del directorio en el que me encuentro 
 - `cd <directorio>` Para meterme en el fichero/carpeta 
 - `cd ..` Para ir al directorio jerarquicamente anterior 

# CLASE 1: COMANDOS ÚTILES EN Git
## Comandos generales
 - `git help`
 - `git status` 
## Comandos para trabajar con ramas
 - `git branch` Ver todas lasa ramas del proyecto 
 - `git branch [nueva_rama]` Crear nueva rama 
 - `git checkout -b [nueva_rama]` Crear nueva rama y cambiar de rama 
 - `git checkout [nombre_rama]` Cambiar a otra rama 
 - `git pull origin main`Hace pull desde la rama main en el origin al repositorio local

# CLASE 2: COMANDOS ÚTILES EN DOCKER
## Comandos básicos en la Client
 - `docker build` Para construir una una imagen de local
 - `docker pull` Para cargar una imagen del Registry público y deja una copia de esa imagen en local
 - `docker run` Para lanzar un contenedor a partir de una imagen
 - `docker ps`Da información de los contenedores que hay corriendo
 - `docker ps -a` Da información de todos los contenedores que tengo, estén corriendo o no
 - `docker images` Da información sobre las imágenes existentes en la máquina
## Comandos específicos en la Client
 - `docker run --name ubuntu-apache2 --rm -d -p 80:80/tcp bsm-ubuntu-apache2:latest` El nombre del contenedor es ubuntu-apache2, --rm borra el contenedor cuando lo cierre (para que no se quede funcionando), -d detacha la terminal del contenedor (lo arranqca en segundo plano, contratio a -it), configuramos una red y le decimos que  el puerto 80 del contenedor lo saque al puerto 80 del a máquina donde está , y que arranque el contenedor a partir de la imagen con nombre:tag bsm-ubuntu-apache2:latest
 - `docker image inspect <nombre_imagen:tag_imagen>` Da toda la información sobre una imagen en particular
## Comandos básicos para crear una imagen
### Imagen Base
`FROM node:16` El dockerfile siempre empieza por la imagen base (primera capa). Se puede poner cualquuier imagen que esté en un Registry privado o púilico. Puedo poner from Ubuntu, o puedo poner from node (un Ubuntu con node instalado dentro)
### Metadatos
`LABEL maintainer info @eloyvalderadev.com` Los metadatos ayudan a idenbtificar ese contenedor.Las buenas prácticas indican quien mantienen esa imagen de ese contenedor con el comando LABEL.
### Variables
`ENV APP_HOME /usr/src/app` Se pueden definir varialbes de entorno a nivel del sistema operativo. En este caso se le indica que el HOME de la aplicación está en el directorio app
### Inicialización
- `WORKDIR /usr/src/app`
- `VOLUME ["/data"]`
- ADD y COPY copian del directorio donde estamos y lo meten en el contenedor
- `ADD file.xyz /file.xyz`
- `COPY --chown=user:group host_file.xyz /path/container_file.xyz`
- `COPY package*.json ./`
### Comandos para especificar al contenedor cómo tiene que arrancar
- `EXPOSE 8080` Puerto del contenedor por el que se expone la funcionlidad del servicio 
- `CMD [ "node", "server.js"]` Indica el comando que arranca dicho servicio. Hay otras variaciones de sintaxis de este comando con ENTRYPOINT
- `ENTRYPOINT [ "node", "server.js" ]`
- `ENTRYPOINT node server.js`
### Recomendación de buenas prácticas
- La intención es crear un contenedor ligero para que seas muy rápido y fácil de mover de un sitio a otro.
- Se puede crear un fichero .dockerignore en el mismo directorio para evitar que se copien ficheros que no deseemos (como logs o librerías).

## Comandos básicos para Docker Volumes
### Gestión de Volumes
1. **Creamos un volumen en Docker**
 - `docker volume create <nombre_volumen>`
2. **Arrancamos el contenedor usando el volumen creado**
 - `docker run -it --name <nombre_contenedor> -v <nombre_volumen>:<directorio_contenedor> centos /bin/bash` El short command -it es para indicarle que mantenga vivo el contenedor mientras lo lanza.
 - `docker run -it --name < nombre_contenedor> --mount source=<nombre volumen>, target=<directorio_contenedor> centos /bin/bash` Otra sintaxis más larga y clara
- **Otros comandos para gestionar volumenes**
  - `docker volume ls` Listar los volumenes creados
  - `docker volume inspect [volume_name]` Inspeccionar un volumen
  - `docker volume rm [volume_name]` Borrar un volumen creado
- Para borrar un volumen es necesario que ningún contenedor esté usando ese volumen
### Bind Mount / tmpfs
1. **Bind Mount: Arrancamos el contenedor usando un punto de montaje concreto del host**
 - `docker run -it --name <nombre_contenedor> -v <fs_local>:<directorio_contenedor> centos /bin/bash`
 - `docker run -it --name <nombre_contenedor> --mount type=bind,source=<fs_local>/target,target=<directorio_contenedor> centos /bin/bash`
2. **TMPFS: Arrancamos el contenedor con la opción tmpfs o usando la opción de mount type tmpfs**
 - `docker run -it --name <nombre_contenedor> --tmpfs /app centos /bin/bash`
 - `docker run -it --name <nombre_contenedor> --mount=tmpfs,destination=/app centos /bin/bash`
