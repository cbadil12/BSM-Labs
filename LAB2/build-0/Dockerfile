#DEsde la imagen base de Ubuntu
FROM ubuntu
#Actualiza paquetería
RUN apt update

ARG DEBIAN_FRONTEND=noninteractive

#Instalamos Apache 2 y su paquete utils
RUN apt install apache2 -y
RUN apt install apache2-utils -y
RUN apt clean

#Configura Apache 2 con variables de entorno
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2

#Indicamos como funciona el contenedor (en el puerto 80 y con el comando apache2ctl)
EXPOSE 80
CMD ["apache2ctl", "-D", "FOREGROUND"]