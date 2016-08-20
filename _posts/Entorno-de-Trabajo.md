# Editor de Texto

Utilizamos [janus](https://github.com/carlhuda/janus), una distribución de [vim](). Primero tenemos que instalar vim  

    $ sudo apt-get install vim curl git ruby rake exuberant-ctags ack-grep
    $ curl -L https://bit.ly/janus-bootstrap | bash

# Rails

## Dependencias

    $ sudo gem install graphviz

## Ruby con rvm

    $ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
    $ curl -L https://get.rvm.io | bash -s stable --autolibs=enabled
    $ rvm install ruby-2.2.0
    $ rvm use ruby-2.2.0

## Bundler

Para trabajar en rails

    $ sudo gem install bundler

también se debe instalar sin sudo en la carpeta de la aplicación

# Ionic

## Dependencias

    $ sudo apt-get install g++
    $ sudo apt-get install zlib1g-dev

## Nodejs

Instalamos [node.js](https://nodejs.org/en/download/package-manager/)  

    $ curl --silent --location https://deb.nodesource.com/setup_4.x | sudo bash -
    $ sudo apt-get install --yes nodejs

## Bower

Instalamos [Bower](http://bower.io/)  

    $ sudo npm install -g bower

## Gulp

Instalamos [Gulp]()  

    $ sudo npm install -g gulp

## Sass

Instalamos [node Sass]()  

    $ sudo npm install -g node-sass

## Cordova

    $ sudo npm install -g cordova

## Ionic

Framework para el desarrollo de aplicaciones híbridas  

    $ sudo npm install -g ionic

# Heroku

heroku toolbelt  

    $ wget -O- https://toolbelt.heroku.com/install-ubuntu.sh | sh

# Android SDK

## Dependencias

    $ sudo apt-get install ant
    $ sudo apt-get install openjdk-7jdk

## SDK

Primero toca bajar el
[sdk](https://developer.android.com/sdk/index.html#Other), luego se
descomprime y se guarda en el workspace  

    $ gzip -d android-sdk_r24.4.1-linux.tgz
    $ tar xvf android-sdk_r24.4.1-linux.tar
    $ mv android-sdk-linux ~/workspace/

Luego vamos a la carpeta del sdk y corremos el **SDK Manager**  

    $ cd workspace/android-sdk-linux/
    $ tools/android sdk &

con esta herramienta nos aseguramos del instalar los paquetes
adicionales que se requieren para utilzar el sdk. Estos paquetes son
**Android SDK Platform-tools** y **Android SDK Build-tools**.  

También instalamos las APIs que necesitemos, seleccionando para cada una
de ellas **SDK Platform**, **ARM EABI v7a System Image** y **Google
APIs**.  

En la sección extras escogemos **Android Support Repository**, **Android
Support Library**, **Google Play Services** y **Google Repository**.  

Luego de descargar los archivos seleccionados, incluimos las siguientes rutas en la
variable de entorno PATH en `~/.bashrc`  

    ~/workspace/android-sdk-linux/tools
    ~/workspace/android-sdk-linux/platform-tools

Ahora configuramos el equipo para que reconozca los dispositivos
android por medio del archivo `51-android.rules`.  

    $ sudo vim /etc/udev/rules.d/51-android.rules

En este archivo agregamos la siguiente línea según el fabricante del
dispositivo que vayamos a utilizar  

    SUBSYSTEM=="usb", ATTR{idVendor}=="0bb4", MODE="0666", GROUP="plugdev"

para especificar el fabricante utilizamos el atributo `ATTR{idVendor}`.
Los códigos para cada fabricante están
[acá](https://developer.android.com/tools/device.html#VendorIds).  

Luego de escribir el archivo, podemos correr el comando `adb devices`,
con este comando en nuestros dispositivos conectados nos aparecerá una
pantalla pidiendo que aceptemos la llave del pc y luego de aceptarla,
podremos utilizar el dispositivo.  



