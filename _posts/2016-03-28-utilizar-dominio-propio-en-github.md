---
layout: post
title: "Utilizar dominio propio en GitHub"
date: 2016-03-28 20:26:04 -0500
categories: web
---

Si tenemos un dominio propio, podemos apuntarlo a la página de GitHub,
para que esta se pueda acceder a través de nuestro propio dominio, y no como un
subdominio de `github.io`

## Configuración DNS

En el [servidor DNS]() se debe agregar una entrada `CNAME` que apunte a
la página de GitHub:

    www.machete.com.co    CNAME    machete-org.github.io

En la mayoría de los casos, esta configuración se debe realizar a través
de un [proveedor de nombres de dominio]().

## CNAME

En el repositorio de la página agregamos un archivo llamado `CNAME` que
contiene el dominio con el que estamos apuntando a nuestra página:

    $ echo www.machete.com.co > CNAME
