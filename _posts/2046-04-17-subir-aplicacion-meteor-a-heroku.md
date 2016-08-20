Vamos a subir a internet la aplicación [Tareas] que se escribió siguiendo el [tutorial de meteor].

		$ git clone https://github.com/lenieto3/tareas.git

## mLab

[mLab](https://mlab.com/) es una plataforma que ofrece bases de datos MongoDB como servicio, y la vamos a utilizar para alojar la base de datos de la aplicación meteor, ya que ofrece 500MB de forma gratuita (solo se puede crear una base de datos). 

Para poder utilizar mLab, debemos [crear una cuenta](https://mlab.com/signup)

## Base de datos

Creamos la base una base de datos para nuestra aplicación.

![botón crear base de datos]()

![información crear base de datos]()

Luego de crear la base de datos, creamos un usuario

![base de datos]()

![usuario]()

## Heroku

[Heroku](https://www.heroku.com/) es una plataforma que permite subir aplicaciones a internet para ponerlas a disposición de sus usuarios.

Utilizamos esta plataforma porque permite actualizar las aplicaciones fácilmente utilizando git, y además ofrece la oportunidad de probar prototipos de forma gratuita.

Para poder utilizar Heroku, debemos [crear una cuenta](https://signup.heroku.com/login).

## Heroku Toolbelt

Luego tenemos que instalar el [cliente heroku](https://toolbelt.heroku.com/), herramienta que nos permite interactuar con la platforma desde la consola.

		$ wget wget -O- https://toolbelt.heroku.com/install.sh | sh

## Crear aplicación en Heroku

Vamos a la carpeta donde tenemos la aplicación

		$ cd workspace/tareas

Iniciamos sesión en Heroku con nuestro correo electrónico y la contraseña de la cuenta en Heroku.

		$ heroku login

Creamos nuestra aplicación **tareas-app** utilizando un [paquete de compilación](https://github.com/seviu/heroku-buildpack-meteor/) hecho especialmente para aplicaciones meteor

		$ heroku create tareas-app --buildpack https://github.com/seviu/heroku-buildpack-meteor.git

Obtenemos la URL de nuestra aplicación

		https://tareas-app.herokuapp.com/ 

Y un repositorio en Heroku

		https://git.heroku.com/tareas-app.git

## Configurar aplicación

Para que nuestra aplicación se pueda conectar con la base de datos que creamos en mLab, le decimos a heroku cuáles son los datos de conexión.

![datos de conexión mLab]()

		$ heroku config:set MONGO_URL=mongodb://<usuariodb>:<password>@ds036562.mongolab.com:26189/<dbname>

El usuario y la contraseña son los datos del usuario que creamos en la base de datos.

Establecemos la URL principal de nuestra aplicación

		$ heroku config:set ROOT_URL=https://tareas-app.herokuapp.com

## Subir aplicación

		$ git push heroku

n614UC3e6YP54wxvalJp
