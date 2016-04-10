---
layout: post
title: "Montar un blog con Jekyll y GitHub"
date: 2016-03-19 10:26:04 -0500
categories: web
---

## Jekyll

Instalamos la gema [Jekyll](http://jekyllrb.com/):  

    $ gem install jekyll

Creamos la carpeta del blog utilizando la gema instalada:  

    $ jekyll new machete-org.github.io

Entramos a la carpeta del blog y ejecutamos el servidor de jekyll para
verlo:  

    $ cd machete-org.github.io
    $ jekyll serve

Ahora podemos ver el blog que hemos creado utilizando un navegador en la
dirección `http://localhost:4000`.  

## Configuración

Jekyll utiliza el archivo `_config.yml` para establecer la configuración
principal del blog.  

En este archivo podemos indicar el título del blog, la url y otras
variables de configuración:  


    # ¡Bienvenido a Jekyll!
    #
    # Este archivo de configuración está hecho para configuraciones que
    # afectan todo el blog, valores que se van a establecer una vez y
    # raramente se cambian.
    # Por razones técnicas, este archivo **NO** es recargado automáticamente
    # cuando se utiliza `jekyll serve`. Si cambia este archivo, por favor
    # reinicie el servidor.

    # Configuraciones de sitio
    title: machete
    email: colabore@machete.com.co
    description: > # Esto signifiva ignorar nuevas líneas hasta "baseurl:"
      Este es un blog colaborativo para el estudio de las Tecnologías de la
      Información. Cualquier persona puede colaborar con la construcción de
      este sitio aportando contenidos, ideas o código a través de Github.
    baseurl: "" # La ruta del sitio, ejemplo: /blog
    url: "http://www.machete.com.co" # La dirección base del sitio y el procotolo
    twitter_username: machetehacking
    github_username: machete-org

    # Build settings
    markdown: kramdown

Una vez terminamos la edición del archivo de configuración reiniciamos
el servidor de Jekyll para ver los cambios realizados.  

Detenemos el servidor con `ctrl + c`, lo volvemos a arrancar con
`jekyll serve`, y luego actualizamos la página en el navegador.  

## Primer Post

Para agregar un post a nuestro blog, creamos un archivo de texto escrito
en formato [markdown](http://markdown.es) y lo guardamos en la carpeta `_posts/`.  

El nombre de este archivo debe empezar con la fecha y terminar con
extensión `.md`.  

    _posts/2016-03-19-montar-un-blog-con-jekyll-y-github.md

El contenido de este archivo debe empezar con una cabecera en la que se
le indicará a Jekyll algunas características del post, como el título,
la fecha y categorías.  

    ---
    layout: post
    title: "Montar un blog con Jekyll y Github"
    date: 2016-03-19 10:26:04 -0500
    categories: web
    ---

La fecha y hora que se indica en la cabecera es la fecha de publicación, por lo que el post
no será visible antes.  

## Primera página

Para agregar una página a nuestro blog, creamos un archivo de texto
escrito en formato markdown y lo guardamos en la carpeta principal:  

    colabore.md

Este archivo también tendrá una cabecera por medio de la cual
definiremos sus características, entre ellas el nombre de la página y
la url:  

    ---
    layout: page
    title: Colabore
    permalink: /colabore/
    ---

## GitHub

Para publicar nuestro blog en internet usaremos el servicio [Páginas GitHub
](https://pages.github.com/). Este servicio nos permite tener una
página estática online de forma gratuita.  

Debemos [abrir una cuenta](https://github.com/join) en GitHub y [crear un repositorio](https://github.com/new) cuyo
nombre sea `[nombre de usuario].github.io`.  

## Gema Páginas GitHub

Para facilitar la integración de nuestro blog con el servicio de páginas
de GitHub, vamos a utilizar la gema `github-pages`.  

Creamos el archivo `Gemfile` en la carpeta del blog y especificamos la
gema que vamos a utilizar en él.  

    source 'https://rubygems.org'
    gem 'github-pages'

Para instalar la gema y configrurar Jekyll para las páginas GitHub utilizamos `bundle`:  

    $ bundle install
    $ bundle exec jekyll build --safe

Corremos el servidor Jekyll y verificamos que nuestro blog funcione
correctamente  

    $ jekyll serve

## Repositorios

Para poder subir nuestro blog a GitHub debemos iniciar un repositorio
local y asociarlo con el repositorio creado en GitHub.  

Iniciamos el repositorio local en la carpeta de nuestro blog:  

    $ git init

Configuramos la información de usuario del repositorio:  

    $ git config user.name "lenieto3"
    $ git config user.email "lenieto3@machete.com.co"

Creamos el commit inicial con todos los archivos del blog:  

    $ git add -A
    $ git commit -m "Comienzo"

Asociamos con el repositorio remoto creado en GitHub:  

    $ git remote add origin https://github.com/machete-org/machete-org.github.io.git

Subimos el código de nuestro blog a GitHub:  

    $ git push -u origin master

## Resultado

Ahora tenemos nuestro blog guardado en nuestro disco como un repositorio local en la
carpeta `machete-org.github.io`. Tenemos una copia de su código fuente subida a
GitHub en `https://github.com/machete-org/machete-org.github.io`. Y la página
web `http://machete-org.github.io` para acceder al blog en internet.  

## Forma de Uso

Para agregar contenido o hacer modificaciones en nuestro blog realizamos
el siguiente procedimiento:  

  1. Realizamos las modificaciones o agregamos los contenidos en nuestra
  copia local del blog, en la carpeta `machete-org.github.io`.  
  2. Utilizamos `jekyll serve` para revisar los cambios que estamos
  haciendo en el blog de manera local.  
  3. Una vez damos por terminados las modificaciones realizadas al blog de
  manera local, hacemos un **commit** para conservar estos cambios.  
  4. Subimos los cambios del repositorio local al repositorio en GitHub
  para que se vean reflejados en la página en internet.

