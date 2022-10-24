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
## Ejemplo para crear una imagen en NODE
`FROM node:16`
### Create app directory
`WORKDIR /usr/src/app`
### Install app dependencies
- A wildcard is used to ensure both package.json AND package-lock.json are copied where available (npm@5+)
`COPY package*.json ./`
`RUN npm install`
- If you are building your code for production `RUN npm ci --only=production`
### Bundle app source
```
COPY ./app /usr/src/app
EXPOSE 8080
CMD [ "node", "server.js" ]
```