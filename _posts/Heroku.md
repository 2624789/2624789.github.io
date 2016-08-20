# Heroku

Verificamos que `heroku` esté instalado

    $ heroku version

Creamos una cuenta en `heroku.com` e iniciamos sesión

    $ heroku login

Generamos una llave pública para autenticarnos en Heroku con `ssh-keygen` y la guardamos en `~/.ssh` y luego la agregamos

    $ heroku keys:add

Ahora creamos la aplicación y le ponemos un nombre

    $ heroku create api-vecino

Y finalmente hacemos el commit y subimos la aplicación a producción utilizando `git`. A Heroku solo se sube la rama `master`.
