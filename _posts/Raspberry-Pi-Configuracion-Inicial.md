## Descargar Raspbian

Descargar imagen  

    https://www.raspberrypi.org/downloads/raspbian/

verificar el hash  

    $ sha1sum 2015-09-24-raspbian-jessie.zip

si coincide, descomprimimos para instalarla  

    $ unzip 2015-09-24-raspbian-jessie.zip
    $ rm 2015-09-24-raspbian-jessie.zip

## Escribir Imagen en SD

Linux Mint monta automáticamente las unidades cuando éstas se conectan,
por lo que tenemos que desmontarlas primero  

    $ sudo umount /media/xx/c36f69c3-56d7-4b82-b2b2-5e551a335d84
    $ sudo umount /media/xx/CC22-47C5

La ruta de montaje depende del sistema operativo que se use, pero debe
ser algo parecido a las anteriores.  

Antes de escribir la imagen en la SD debemos identificar la ruta del
dispositivo  

    $ sudo fdisk -l

    Disk /dev/mmcblk0: 7948 MB, 7948206080 bytes

Para escribir la imagen utilizamos la herramienta `dd`  

    $ sudo dd bs=4M if=2015-09-24-raspbian-jessie.img of=/dev/mmcblk0

este proceso puede demorar varios minutos.  

Cuando finalice vaciamos el caché de escritura y luego retiramos la SD  

    $ sync

Volvemos a conectar la SD y Linux Mint de forma automática montará las
dos particiones que se han creado.  

Una de estas particiones es la de inicio `boot` y la otra es la que
contiene el sistema de archivos.  

## Configurar Red

Para configurar la forma en que la Raspberry se conectará a la red
tenemos que editar archivos en el directorio `etc/`. Como esta partición
está montada, accedemos a ella a través de la ruta de montaje (esta ruta
puede variar según la distribución utilizada y la forma de montaje de la
SD  

    $ sudo vim /media/xx/ec2cba/etc/network/interfaces

En este archivo establecemos la dirección IP estática que usará la
Raspberry en la interfaz de red inalámbrica  

    auto wlan0
    allow-hotplug wlan0
    iface wlan0 inet static
    address 192.168.0.21
    gateway 192.168.0.1
    netmask 255.255.255.0
    network 192.168.0.0
    broadcast 192.168.0.255
    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf

Estos datos dependen de la red local a la que tenemos acceso.  

Luego ingresamos la información de conexión para la red inalámbrica  

    $ sudo vim /media/xx/ec2cba/etc/wpa_supplicant/wpa_supplicant.conf

    network={
      ssid="El nombre de la red"
      psk="L@ cl4v3 de la R3d"
    }

Cuando terminemos de editar estos archivos desmontamos las particiones
de la SD, la retiramos y la ponemos en la Raspberry con el Wi-Fi usb.  

## Conexión SSH

Luego de conectar la Raspberry a una fuente de energía, esta iniciará y
se conectará automáticamente a la red y podemos establecer la conexión
ssh  

    $ ssh pi@192.168.0.21

la contraseña es `raspberry`.  

## Raspi-Config

Luego de acceder de forma remota a la Raspberry corremos la herramienta
de configuración raspi-config  

    $ sudo raspi-config

Configuramos la raspberry para que no inicie el modo gráfico, ya que
solo la vamos a utilizar a través de la conexión ssh  

    Boot Options -> B1 Console
    Expand Filesystem

Finalizamos y reiniciamos la Raspberry.  

## Actualización

Actualizamos el software de la Raspberry  

    $ sudo apt-get update && sudo apt-get upgrade

## Vim Janus

Primero instalamos el editor de texto [vim]()  

    $ sudo apt-get install vim

Después instalamos algunos paquetes necesarios para compilar de forma
correcta la distribución de vim que vamos a utilizar  

    $ sudo apt-get install rake git

Luego instalamos la distribución [janus](https://github.com/carlhuda/janus)  

    $ curl -L https://bit.ly/janus-bootstrap | bash

## Python3

Para utilizar por defecto la versión 3 de Python, creamos un alias en
`~/.bashrc`  

    alias python='python3'
