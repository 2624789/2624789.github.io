---
layout: post
title: "Instalar Wordpress en Ubuntu"
date: 2017-01-20 10:26:04 -0500
categories: web
---

WordPress es un sistema de gestión de contenidos o CMS (por sus siglas en inglés, Content Management System) enfocado a la creación de cualquier tipo de sitio. Originalmente alcanzó una gran relevancia usado para la creación de blogs, para convertirse con el tiempo en una de las principales herramientas para la creación de páginas web comerciales. Ha sido desarrollado en el lenguaje PHP para entornos que ejecuten MySQL y Apache, bajo licencia GPL y es software libre. [Wikipedia](https://es.wikipedia.org/wiki/WordPress)

# Requerimientos

Para que WordPress funcione se debe contar con un **servidor web** ([Apache]()) que pueda procesar código ([PHP]()) y un **servidor de bases de datos** ([MySQL]()).

## Servidor web

Instalamos el servidor web Apache:

    $ sudo apt install apache2 apache2-utils

Utilizamos `systemctl` para habilitar el servicio cada vez que se inicie
el equipo (**enable**), o simplemente lo arrancamos (**start**):

    $ sudo systemctl enable apache2
    $ sudo systemctl start apache2

Con `systemctl` también podemos ver el estado del servicio:

    $ sudo systemctl status apache2

Podemos verificar el puerto que está utilizando el servicio con `netstat`:

    $ sudo netstat -anltp | grep "apache2"

Ahora podemos acceder a la página de inicio del servidor web utilizando un navegador para visitar la url `localhost`.

Cuando visitamos esta url, el servicio apache se encarga de darnos acceso al directorio `/var/www/html`. En el momento de la instalación del servidor web en esta carpeta se encuentra un archivo plano llamado `index.html` que corresponde a una página de prueba del servidor con información relevante acerca de su configuración.

![página de prueba apache](http://i.imgur.com/PwvAve4.png)

El propósito de este archivo es únicamente validar el funcionamiento del servidor web, una vez hecho esto se recomienda borrarlo.

## Servidor de bases de datos

Instalamos el servidor MySQL y un cliente para accederlo directamente:

    $ sudo apt install mysql-client mysql-server

Durante la instalación de estos paquetes se solicita establecer la contraseña del usuario `root` del servidor de base de datos. Se debe configurar una contraseña y guardarla.

De la misma manera como verificamos el estado del servicio con `systemctl `y el puerto utilizado con `netstat` para el servidor web, podemos hacerlo para el servidor `mysql`.

    $ sudo systemctl disable mysql
    $ sudo systemctl enable mysql
    $ sudo systemctl start mysql
    $ sudo systemctl status mysql
    $ sudo netstat -anltp | grep "mysql"

Para acceder al servicio, utilizamos el cliente `mysql` que instalamos. Indicamos que queremos conectarnos como el usuario `root` e ingresamos la contraseña que configuramos durante la instalación, lo cual nos permite acceder por consola al servicio de base de datos.

    $ mysql -u root -p
    Enter password: 
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 4
    Server version: 5.7.16-0ubuntu0.16.10.1 (Ubuntu)

    Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights
    reserved.

    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.

    Type 'help;' or '\h' for help. Type '\c' to clear the current input
    statement.

    mysql> quit
    Bye

## PHP

PHP es un lenguaje de programación utilizado para escribir aplicaciones web. PHP permite generar páginas HTML de forma dinámica y mostrarlas a través del servidor web, establecer conexiones con bases de datos y ejecutar scripts.

A continuación instalamos los paquetes necesarios para procesar código PHP.

    $ sudo apt install php7.0 php7.0-mysql libapache2-mod-php7.0 php7.0-cli php7.0-cgi php7.0-gd php7.0-xml

Para probar que el servidor web interpreta correctamente PHP, creamos el archivo `/var/www/html/info.php` con la siguiente línea en PHP.

    <?php phpinfo(); ?>

y lo accedemos en la url `localhost/info.php` utilizando un navegador. Podemos ver que el servidor web interpreta la función `phpinfo()` y genera una página donde se muestra la versión de php instalada y otros datos de configuración.

![página de prueba php](http://i.imgur.com/Ou7rzw0.png)

El propósito de este archivo es únicamente validar la instalación de PHP, una vez hecho esto se recomienda borrarlo.

# Wordpress

## Instalación

Para instalar WordPress descargamos la última versión desde la página oficial, la descomprimimos y ubicamos su contenido en la carpeta del servidor web `/var/www/html`.

    $ wget -c http://wordpress.org/latest.tar.gz
    $ tar -xzvf latest.tar.gz
    $ sudo rsync -av wordpress/* /var/www/html/

Luego le asignamos a los archivos los permisos adecuados:

    $ sudo chown -R www-data:www-data /var/www/html/
    $ sudo chmod -R 755 /var/www/html/

## Base de datos

WordPress necesita una base de datos para funcionar, es por esto que debemos conectarnos al servidor `mysql` y crearle un usuario y una base de datos.

    $ mysql -u root -p

    mysql> CREATE DATABASE wordpress;
    mysql> CREATE USER wordpress@localhost IDENTIFIED BY 'password';
    mysql> GRANT ALL PRIVILEGES ON wordpress.* TO wordpress@localhost;
    mysql> FLUSH PRIVILEGES;
    mysql> quit

## Configuración

Debemos crear el archivo de configuración de WordPress e ingresar en él la información necesaria para conectarse a la base de datos. Utilizamos como punto de partida el archivo de muestra que se descargó en la instalación.

   $ sudo mv /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
   $ sudo vim /var/www/html/wp-config.php

En ingresamos los datos de conexión:

    // ** MySQL settings - You can get this info from your web host ** //
    /** The name of the database for WordPress */
    define('DB_NAME', 'wordpress');

    /** MySQL database username */
    define('DB_USER', 'wordpress');

    /** MySQL database password */
    define('DB_PASSWORD', 'password');

    /** MySQL hostname */
    define('DB_HOST', 'localhost');

    /** Database Charset to use in creating database tables. */
    define('DB_CHARSET', 'utf8');

    /** The Database Collate type. Don't change this if in doubt. */
    define('DB_COLLATE', '');

Luego reiniciamos los servicios web y base de datos:

    $ sudo systemctl restart apache2
    $ sudo systemctl restart mysql

Ahora volvemos a acceder a la url `localhost` desde el navegador y automáticamente nos redirecciona a ``, donde encontramos una página donde podemos ingresar el nombre del sitio web y los datos del usuario administrador de wordpress:

![página de bienvenida wordpress](http://i.imgur.com/tOzFdfI.png)

Luego de ingresar los datos damos click en el botón `Install WordPress` y al cabo de unos segundos, tenemos acceso al panel de control de nuestro sitio web.
