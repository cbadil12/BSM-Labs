# PRÁCTICA 1: Git
## Crear cuenta en GitHub y crear un repositorio
- https://github.com/
## Configurar git paar que utilice mi usuario y contraseña
- `git config --global user.ame "Carlos Luis Badillo"`
- `git congig --global user.email cbadil12@gmail.com`
## Crear mi primer repositorio local y subirlo a GitHub
### Se inicializa el repositorio
- `git init` <br
- `git remote add origin https://github.com/cbadil12/BSM-Labs.git`
### Se crea el fichero README.md
- `echo "# BSM-Labs" >> README.md`
### Se actualizar el repositorio
- Primero se guarda el archivo con CTRL+S
- `git add LAB1` Añade el fichero LAB1 a la zona de staging
- `git commit -m "Comentario del commit"` Confirma el cambio
- `git push -u origin main` Sincroniza el repositorio en la rama main
- `git push -u origin dev` Sincroniza el repositorio en la rama dev

# COMANDOS ÚTILES EN Git
## Comandos generales
`git help`
`git status` 
## Comandos para trabajar con ramas
`git branch` Ver todas lasa ramas del proyecto 
`git branch [nueva_rama]` Crear nueva rama 
`git checkout -b [nueva_rama]` Crear nueva rama y cambiar de rama 
`git checkout [nombre_rama]` Cambiar a otra rama 
`git pull origin main`Hace pull desde la rama main en el origin al repositorio local

## Comandos básicos en la terminal
`ls` Lista el contenido del directorio en el que me encuentro 
`cd <directorio>` Para meterme en el fichero/carpeta 
`cd ..` Para ir al directorio jerarquicamente anterior 

# PRÁCTICA 2: Docker
## Imágenes de Docker
Las imágenes son ficheros estáticos inmutables que inluye la aplicacion y todas sus dependencias, de esta forma es portable e independiente de la infraestructura.<br />
Las imágenes son construidas por diferentes capas sobre una imagen base. Estas capas permiten reutilizar componentes para otras construcciones.
## Comandos básicos en la Client
- `docker build` Para construir una una imagen de local
- `docker pull` Para cargar una imagen del Registry público y deja una copia de esa imagen en local
- `docker run` Para lanzar un contenedor a partir de una imagen
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
## Recomendación de buenas prácticas
- La intención es crear un contenedor ligero para que seas muy rápido y fácil de mover de un sitio a otro.
- Se puede crear un fichero .dockerignore en el mismo directorio para evitar que se copien ficheros que no deseemos (como logs o librerías).

## Docker Volumes
- Un contenedor por esencia es efímero. todos los datos que se generen durante el proceso desaparecerán si borro el proceso.
- En ocasiones es necesario que nuestras aplicaciones tengan datos persistentes. Como por ejemplo una base de datos.
- Existen diferentes formas de almacenar esta información según el caso de uso y nuestras necesidades:
  - Volume
  - Bind Mount
  - Tmpfs mount