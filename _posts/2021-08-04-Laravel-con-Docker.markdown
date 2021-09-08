---
title: "Laravel con Docker"
layout: post
date: 2021-08-04 22:44
image: /assets/images/laravel-docker-cover.png
headerImage: true
tag:
- linux
- php
- laravel
- docker
- nginx
star: true
category: blog
author: febrero
description: Montamos un proxecto de larevel con contenedores docker
---

# LARAVEL con Docker

Creamos un proxecto con Laravel usando Docker como entorno de traballo. Usaremos Laravel 8, MySQL 5.7 e Nginx.

## Intalacion de Composer

Para crear un proxecto de laravel necesitaremos **Composer**, **Composer** realiza a instalacion de todos os paquetes laraavel coas suas versions compatibles. Teremos que ter instalado php para poder realizar a instalacion de **Composer**.

{% highlight raw %}
### instalacion php
sudo apt update
sudo apt -y install php7.4

### instalacion Composer
php -r "copy('https://getcomposer.org/installer', '/tmp/composer-setup.php');"
 
php -r "if (hash_file('sha384', '/tmp/composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('/tmp/composer-setup.php'); } echo PHP_EOL;"
 
sudo php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer
 
php -r "unlink('composer-setup.php');"

{% endhighlight %}

## Creamos o proxecto con laravel

Creamos o proxecto con laravel mediante o Composer. Estructura comando: **Composer** seguuido de  **Create-project** seguido do tipo de proxecto **Laravel/Laravel** e por ultimo o nome do noso proxecto.

{% highlight raw %}
composer create-project laravel/laravel casaruraldb

{% endhighlight %}


## Creaccion do Dockerfile

Dentro da carpeta que creamos, crearemos o Dockerfile da maquina con php.

{% highlight raw %}
cd casaruraldb
subl Dockerfile
{% endhighlight %}

### Dockerfile

{% highlight raw %}
## Dockerfile
FROM php:7.4-fpm

#### Permitimos el paso de parámetros (argumentos) que se definirán en el fichero docker-compose.yml
 ARG user
 ARG uid

#### Añadimos dependencias y utilidades interesantes al sistema como: git, curl, zip, …:
RUN apt-get update 
RUN apt-get install -y \
    git \
    curl \
    libxml2-dev \
    libonig-dev \
    libpng-dev \
    zip \
    unzip \
    util-linux

#### Una vez finalizado borramos cache y limpiamos los archivos de instalación
RUN apt-get clean 
RUN rm -rf /var/lib/apt/lists/*

#Instalamos las dependencias y extensiones PHP que necesitaremos en nuestro proyecto como: pdo_mysql o mbstring
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd sockets 

#### Instalamos dentro de la imagen la última versión de composer, para ello copiamos la imagen disponible en el repositorio:
COPY --from=composer:2.0.13 /usr/bin/composer /usr/bin/composer

 
#### Copiamos de la última imagen de node en nuestro proyecto las librerías de los módulos y de node
COPY --from=node:latest /usr/local/lib/node_modules /usr/local/lib/node_modules
COPY --from=node:latest /usr/local/bin/node /usr/local/bin/node

#### Creamos un enlace virtual para poder utilizar directamente npm dentro de la máquina Docker:
RUN ln -s /usr/local/lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm

#### Creamos un usuario de sistema para ejecutar los comando Composer y Artisan:
RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer 
RUN chown -R $user:$user /home/$user
 
#### Definimos el directorio de trabajo dentro de nuestra imagen
WORKDIR /var/www
USER $user



{% endhighlight %}

---

# Configuracion de Nginx

Dentro do noso proxecto crearemos una carpeta chamada Docker-compose e dentro desta 2 carpetas mais chamadas nginx e mysql.

Dentro da carpeta nginx crearemos o ficheiro de configuracion do Nginx, chamamoslle casarural-server-conf, coa seguinte configuracion:

{% highlight raw %}
server {
     listen 80;
     index index.php index.html;
     error_log  /var/log/nginx/error.log;
     access_log /var/log/nginx/access.log;
     root /var/www/public;
     location ~ .php$ {
         try_files $uri =404;
         fastcgi_split_path_info ^(.+.php)(/.+)$;
         fastcgi_pass app:9000;
         fastcgi_index index.php;
         include fastcgi_params;
         fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
         fastcgi_param PATH_INFO $fastcgi_path_info;
     }
     location / {
         try_files $uri $uri/ /index.php?$query_string;
         gzip_static on;
     }
 }
{% endhighlight %}

# Configuracion de Docker-compose.yml

No docker-compose.yml configuramos os contenedores necesarios para a configuracion do noso entorno en laravel, no noso casa tera o contendor principal APP que cargara o dockerfile creado anteriormente, unha imaxen de MySQL e unha imaxen de nginx:alpine.

{% highlight raw %}
server {
     version: "3"
services:
   app:
     build:
       args:
         user: javier
         uid: 1000
       context: ./
       dockerfile: Dockerfile
     image: casaruraldb
     container_name: casaruraldb-app
     restart: unless-stopped
     working_dir: /var/www/
     volumes:
       - ./:/var/www
     networks:
       - net-casaruraldb
   db:
     image: mysql:5.7
     container_name: casaruraldb-db
     restart: unless-stopped
     ports:
      - 127.0.0.1:23306:3306
     environment:
       MYSQL_DATABASE: ${DB_DATABASE}
       MYSQL_ROOT_PASSWORD: ${DB_PASSWORD}
       MYSQL_PASSWORD: ${DB_PASSWORD}
       MYSQL_USER: ${DB_USERNAME}
       SERVICE_TAGS: dev
       SERVICE_NAME: mysql
     volumes:
       - ./docker-compose/mysql:/docker-entrypoint-initdb.d
       - ../casaruraldb-db/var/lib/mysql:/var/lib/mysql
     networks:
       - net-casaruraldb
   nginx:
         image: nginx:alpine
         container_name: casaruraldb-nginx
         restart: unless-stopped
         ports:
           - 127.0.0.1:28000:80
         volumes:
           - ./:/var/www
           - ./docker-compose/nginx:/etc/nginx/conf.d/
         networks:
           - net-casaruraldb
networks:
   net-casaruraldb:
     driver: bridge
{% endhighlight %}

## Contruiremos os contendores

{% highlight raw %}
sudo docker-compose build
{% endhighlight %}

## Levantamos os contenedores

{% highlight raw %}
sudo docker-compose up
{% endhighlight %}

## Configuracion da nosa APP
 Accedemos dentro do contendor da nosa APP e generamos a clave da nosa APP.


{% highlight raw %}
sudo docker exec -it app-id bash
{% endhighlight %}

### Dentro do contenedor APP
{% highlight raw %}

### Generamos a clave
php artisan key:generate

### Realizamos un composer install
composer install

### Realizamos un npm install
npm install

{% endhighlight %}

---

---
Referencias
[Docker Hub PHO][1]
[Docker Hub MySQL][2]
[Docker Hub Nginx][3]
[Codigo Xules][4]
[Laravel Docs][5]
---


[1]: https://hub.docker.com/_/php
[2]: https://hub.docker.com/_/mysql
[3]: https://hub.docker.com/_/nginx
[4]: https://codigoxules.org/docker-laravel-configuracion-e-instalacion-de-un-proyecto-con-laravel/
[5]: https://laravel.com/docs/8.x
