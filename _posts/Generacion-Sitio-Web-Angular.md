# Generador Angular Yeoman

Para iniciar el sitio web utilizamos el generador [angular](https://github.com/yeoman/generator-angular#readme) de  [Yeoman](http://yeoman.io/).

Utilizar un generador nos permite configurar de forma automática una
página web utilizando [grunt](http://gruntjs.com/) para automatizar las tareas repetitivas y
[bower](http://bower.io/) para la gestión de paquetes, teniendo en cuenta buenas
prácticas.

Primero creamos la carpeta donde vamos a guardar los archivos del sitio
y luego lo generamos

	$ mkdir webapp
	$ cd webapp
	$ yo angular

cuando generamos el sitio indicamos que se incluya la versión sass de bootstrap y algunos módulos angular (angular-animate.js, angular-cookies.js, angular-sanitize.js, angular-touch.js).

Para probar en local utilizamos `grunt serve` y para correr los tests con karma `grunt test`.


# Repositorios

Crea repositorios remotos y asociar

# Configuración incial

Después de generar el sitio web realizamos unas modificaciones básicas
en el index y agregamos un favicon para irlo ajustando a las necesidades
específicas del proyecto.

Desintalamos 'ngResource' porque no lo vamos a usar  

    $ bower uninstall angular-resource --save

Instalamos 'Restmod', 'ui-router', y 'ng-token-auth'  

    $ bower install angular-restmod --save
    $ bower install angular-ui-router --save
    $ bower install ng-token-auth --save
    $ bower install sweetalert --save

cambiamos el nombre del módulo por `app` y lo actualizamos en `app.js`.
Almacenamos el módulo como una variable y también agregamos como dependencias los módulos instalados.

En el index agregamos la vista donde vamos a cargar las plantilla del
ui-router

    <div class="container-fluid padding">
      <div ui-view=""></div>
    </div>

Cambiamos los contenedores por 'container-fluid' y le agregamos un
padding al de la vista.

También agregamos al index el js para localizar el app y el formato del
API para 'restmod' (es un fix porque bower no lo agrega solo)

    <script src="scripts/i18n/angular-locale_es-co.js"></script>
    <script src="scripts/ams.min.js"></script>

Luego agregamos las configuraciones de restmod, ng-token-auth y un
estado home de prueba con su plantilla.

    // Inicio
    app.config(function($stateProvider, $urlRouterProvider) {
      $stateProvider

      .state('inicio', {
        url: '/',
        templateUrl: 'views/inicio.html',
        controller: ['$scope',
          function($scope) {
            $scope.inicio = "Admin"
          }
        ]
      })

      // Otherwise
      $urlRouterProvider.otherwise('/')
    })

    // Restmod
    app.config(function(restmodProvider) {
      restmodProvider.rebase('AMSApi')
    })

    // ng-token-auth
    app.config(function($authProvider) {
      $authProvider.configure({
        apiUrl: 'https://api.herokuapp.com',
        storage: 'localStorage'
      })
    })

A continuación agregamos el estado y las vistas para iniciar sesión

    app/scripts/estados/auth.js
    app/views/auth/entrar.html

# Producción

    commit aefc308eba5160c4a28cdbafe2c39199e30d9c62

Para subir la página a producción en Heroku tenemos que crear un archivo `Procfile`, crear el servidor en `web.js` y quitar el directorio `dist/` de `.gitignore` para que sea subido a heroku.

    $ echo "web: node web.js" > Procfile
    $ touch web.js

    // web.js
    var gzippo = require('gzippo');
    var express = require('express');
    var morgan = require('morgan');
    var app = express();

    app.use(morgan('dev'));
    app.use(gzippo.staticGzip("" + __dirname + "/dist"));
    app.listen(process.env.PORT || 5000);

Se deben instalar las dependencias del servidor

    $ npm install gzippo express morgan --save

El directorio `dist/` se genera con grunt

    $ grunt build

Luego hacemos el commit, iniciamos sesión en heroku, creamos el app y luego subimos el repositorio.

    $ git add -A
    $ git commit -m "Configura producción en Heroku"
    $ heroku login
    $ heroku create webapp
    $ git push heroku

**Siempre toca compilar el sitio antes de subirlo a heroku**

**Para probar cómo se ve el sitio web en producción utilizamos `foreman start`**


