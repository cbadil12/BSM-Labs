 # PRÁCTICA 2: Creando los primeros contenedores
 ## 1. Instalar repositorio de Docker
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.
### 1.1 Set up the repository
 - https://docs.docker.com/engine/install/ubuntu/
 - Update the `apt` package index and install packages to allow `apt` to use a repository over HTTPS:

   - `sudo apt-get update`Actualiza la paquetería de Ubuntu
   -  ```
       sudo apt-get install \
        ca-certificates \
          curl \
          gnupg \
          lsb-release
      ``` 
      Instala gestor de certificados, comando curl que permite descqargar ficheros de urls, el gestor de paquetes gnupg a nivel de claves y lsb-release que permite saber en que versión del kernel estamos trabajando

### 1.2 Add Docker’s official GPG key:
 - `sudo mkdir -p /etc/apt/keyrings`
 - `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg`
### 1.3 Use the following command to set up the repository:
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
## 2. Instalar Docker Engine
### 2.1 Update the `apt` package index:
  - `sudo apt-get update` Actualiza la paquetería de Ubuntu
### 2.2 Install Docker Engine, containerd, and Docker Compose. For the lastest version, run:
  - `sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin`
### 2.3 Comprobar versión de Docker instalada
- `docker version`Debe ser la versión 20.10.17
### 2.4 Para que el usuario pueda gestionar el motor del contenedor
`sudo usermod -aG docker $USER`

## 3. Instalar extensiones en VisualStudio
 - Docker
 - Kubernetes
 - YAML

 ## 4. Correr imagen (de Ubuntu)
   - `docker run -it --name mi_primera_ubuntu ubuntu /bin/bash`
   - Starteo y exiteo el contenedor
   - `docker ps`Da información de los contenedores que hay corriendo
   - `docker ps -a` Da información de todos los contenedores que tengo, estén corriendo o no
  
  ## 5. Crear imagen desde cero
   - `FROM ubuntu` Para este ejercicio simulamos que tenemos un sistema operativo Ubuntu
   - `RUN ap update` Actualizamos la paquetería de ese sistema operativo

   - `ARG DEBIAN_FRONTEND=noninteractive` Definimos argumento para que no nos pregunte si queremos instalar los paquetes o no
   - `RUN apt install apache2 -y` Instalamos el software Apache, que es un frontal (página web)
   - `RUN apt install apache2 -utils -y` Instalamos el paquete de utilidades "utils" de Apache
   - `RUN apt clean` Limpiamos todos los archivos temporales, ya que se busca que la imagen del contenedor sea lo más ligera posible

   - `ENV APACHE_RUN_USER www-data` Variable de entorno necesaria para que funcione
   - `ENV APACHE_RUN_GROUP www-data` Variable de entorno necesaria para que funcione
   - `ENV APACHE_LOG_DIR /var/log/apache2` Variable de entorno necesaria para que funcione

   - `EXPOSE 80` Arranca el contenedor exponinendo el puerto 80 en el host
   - `CMD ["apache2ctl", "-D", "FOREGROUND"]` Proceso que tiene que arrancar

 MINUTO 2:15

 Esto lo escribo en la rama dev