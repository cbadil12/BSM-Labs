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

## Comandos básicos en la terminal
 - `ls` Lista el contenido del directorio en el que me encuentro 
 - `cd <directorio>` Para meterme en el fichero/carpeta 
 - `cd ..` Para ir al directorio jerarquicamente anterior 

# CLASE 2: DOCKER
## Imágenes de Docker
Las imágenes son ficheros estáticos inmutables que inluye la aplicacion y todas sus dependencias, de esta forma es portable e independiente de la infraestructura.<br />
Las imágenes son construidas por diferentes capas sobre una imagen base. Estas capas permiten reutilizar componentes para otras construcciones.
## Docker Volumes
 - Un contenedor por esencia es efímero. todos los datos que se generen durante el proceso desaparecerán si borro el proceso.
 - En ocasiones es necesario que nuestras aplicaciones tengan datos persistentes. Como por ejemplo una base de datos.
 - Existen diferentes formas de almacenar esta información según el caso de uso y nuestras necesidades:
   - **Volume**: el más normal y aconsejado. Se almacena dentro del FS del host dentro de var/lib/docker/volumes y gestionado por Docker. Otros procesos no podrán modificar estos ficheros. Es la mejor opción de persistir información en Docker.
   - **Bind Mount**: Se puede almacenar información en cualquier FS del host. Otros procesos en este caso sí podrán modificar estos ficheros.
   - **Tmpfs mount**: Permite almacenar información en la memoria del host pero nunca será escrita en el filesystem del host. Estos ficheros sólo estarán disponible durante la vida del contenedor.
 - Independientemente del tipo de almacenamiento que elijamos dentro del contenedor se verá como un directorio/fichero disponible dónde podremos leer y escribir.
## Docker Netwoks
 - Todas las aplicaciones desplegadas van a necesitar comunicar con otras aplicaciones o permitir el acceso al servicio que ofrece.
 - Docker al tratarse de aplicaciones ligeras, minimiza la superficie de ataque exponiendo sólo aquellos puertos que realmente son necesarios, limitando de esta forma que un malware pueda hacer uso de ciertos puertos/protocolos para tomar el control.
 - Existen diferentes configuraciones de la red a nivel de Docker, las más importantes:
   - **Bridge**: El driver por defecto para gestionar las redes de los contenedores de un host. Permite que varios contenedores que están arrancados en la misma máquina, se llamen entre ellos.
   - **Host**: Se elimina el asilamiento entre el host y el contenedor de red. El contenedor pasa a utilizar la propia red del host. El propio contenedor va a poder llamar a los puertos del host.
   - **Overlay**: Permite conectar múltiples demonios de contenedores juntas y permitir servicios distribuidos (cuando hay más de un host).
   - **None**: Se desactiva todas las redes del contenedor, este contenedor sólo podrá hacer computo pero no será accesible ni podrá acceder a ninguna otra aplicación.
## Comandos básicos en la Client
 - `docker build` Para construir una una imagen de local
 - `docker pull` Para cargar una imagen del Registry público y deja una copia de esa imagen en local
 - `docker run` Para lanzar un contenedor a partir de una imagen
 - `docker ps`Da información de los contenedores que hay corriendo
 - `docker ps -a` Da información de todos los contenedores que tengo, estén corriendo o no
## Ejemplo de DOCKERFILE para crear una imagen en NODE
### Le digo que cree el contenedor utilizando la tecnologiade nodejs (sin encesidad de tener una máquina virtual entera de node)
`FROM node:16`
### Crear el directorio de la app, donde nodejs lee la aplicación
`WORKDIR /usr/src/app`
### Install app dependencies
 A wildcard is used to ensure both package.json AND package-lock.json are copied where available (npm@5+)<br />
 - `COPY package*.json ./` Copia todos los package .json al directorio de la app
 - `RUN npm install`
If you are building your code for production `RUN npm ci --only=production`
### Bundle app source
 - `COPY ./app /usr/src/app` Copia la aplicación al directorio app
 - `EXPOSE 8080` Le decimos que esa aplicacion expone el puerto 8080, el puerto donde la aplicación del contenedor está dando servicio
 - `CMD [ "node", "server.js" ]` Cuando arranque el contenedor (se cree una instancia de ese contenedor) tiene que lanzar el comando node con el fichero server.js

https://nodejs.org/en/docs/guides/nodejs-docker-webapp/
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
