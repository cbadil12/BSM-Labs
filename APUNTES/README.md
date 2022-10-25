# CLASE 1: 

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

