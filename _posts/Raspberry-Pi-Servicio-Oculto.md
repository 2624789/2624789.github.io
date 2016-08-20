Los servicios ocultos son servicios web que se pueden acceder a través
de la red [Tor](https://www.torproject.org/), un sistema que permite el
uso de internet de forma anónima.  

## Tor

Primero debemos instalar el paquete tor que ya viene configurado
para ser utilizado como cliente de la red  

    $ sudo apt-get install tor

## Servidor Web Local

Para crear nuestro servicio oculto primero debemos tenerlo corriendo de
manera local.  

Para montar un servidor web local primero instalamos [Apache]()  

    $ sudo apt-get install apache2

Luego lo configuramos para que corra de manera local en
`/etc/apache2/ports.conf`  

    Listen 127.0.0.1:80

Agregamos una página estática  

    $ sudo vim /var/www/html/index.html

Finalmente iniciamos el servicio para activar el servidor  

    $ sudo service apache2 restart

Si queremos garantizar el anonimato de nuestro servicio oculto debemos
configurar el servidor web de tal forma que no revele ninguna
información acerca del usuario, el computador o la ubicación, así como
otras medidas adicionales según la paranoia del servicio.  

## Servicio Oculto

Ahora tenemos que configurar el servicio oculto para que apunte al
servidor web local.  

Esto lo hacemos habilitando las siguientes líneas en `/etc/tor/torrc`  

    HiddenServiceDir /var/lib/tor/hidden_service/
    HiddenServicePort 80 127.0.0.1:80

HiddenServiceDir es el directorio donde Tor guardará información acerca
del servicio oculto. Acá se creará un archivo llamado `hostname` que
contendrá la URL onion y otra información secreta.  

HiddenServicePort permite especificar un puerto virtual a través del
cual se accede el servicio oculto y una dirección IP y puerto para
redirigir las conexiones a este puerto virtual.  

Luego de editar este archivo reiniciamos el servicio tor

    $ sudo service tor restart

Cuando tor reinicia, crea el directorio especificado por
HiddenServiceDir y allí guarda la llave privada del sitio en
`private_key` y la url en `hostname`.  

Para acceder a la página web que se creó se debe utilizar el [Navegador
Tor](https://www.torproject.org/projects/torbrowser.html.en) y acceder a la URL onion de nuestro servicio oculto.  


esncz3shhgxdny2l.onion