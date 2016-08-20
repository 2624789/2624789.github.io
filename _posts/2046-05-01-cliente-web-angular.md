# Generador Angular Yeoman

Para iniciar el sitio web utilizamos el generador [angular](https://github.com/yeoman/generator-angular#readme) de  [Yeoman](http://yeoman.io/).

		$ npm install -g grunt-cli bower yo generator-karma generator-angular

Utilizar un generador nos permite configurar de forma automática una
página web utilizando [grunt](http://gruntjs.com/) para automatizar las tareas repetitivas y
[bower](http://bower.io/) para la gestión de paquetes, teniendo en cuenta buenas
prácticas.

Primero creamos la carpeta donde vamos a guardar los archivos del sitio
y luego lo generamos

	$ mkdir webapp
	$ cd webapp
	$ yo angular

cuando generamos el sitio indicamos que se incluyan algunos módulos angular (angular-cookies.js, angular-sanitize.js).

http://security.stackexchange.com/questions/71878/cordova-phonegap-refreshtoken-in-localstorage

		$ yo angular

		     _-----_
		    |       |    .--------------------------.
		    |--(o)--|    |    Welcome to Yeoman,    |
		   `---------´   |   ladies and gentlemen!  |
		    ( _´U`_ )    '--------------------------'
		    /___A___\    
		     |  ~  |     
		   __'.___.'__   
		 ´   `  |° ´ Y ` 

		Out of the box I include Bootstrap and some AngularJS recommended modules.

		? Would you like to use Gulp (experimental) instead of Grunt? No
		? Would you like to use Sass (with Compass)? Yes
		? Would you like to include Bootstrap? No
		? Which modules would you like to include? 
		 ◯ angular-animate.js
		 ◯ angular-aria.js
		 ◉ angular-cookies.js
		 ◯ angular-resource.js
		 ◉ angular-messages.js
		 ◯ angular-route.js
		❯◉ angular-sanitize.js
		 ◯ angular-touch.js

Para probar en local utilizamos `grunt serve` y para correr los tests con karma `grunt test`.

# Repositorios

Crea repositorios remotos y asociar

# Configuración inicial

Después de generar el sitio web realizamos unas modificaciones básicas
en el index y agregamos un favicon para irlo ajustando a las necesidades
específicas del proyecto.

Instalamos los módulos que vamos a usar:  

    $ bower install angular-restmod angular-ui-router ng-token-auth angular-material --save

cambiamos el nombre del módulo por `app` y lo actualizamos en `app.js`.
Almacenamos el módulo como una variable y también agregamos como dependencias los módulos instalados.

		var app = angular.module('app', [ 
			'ngCookies',
			'ngMessages',
			'ngSanitize',
			'ui.router',
			'restmod',
			'ng-token-auth',
			'ngMaterial'
		]); 

En el index agregamos la vistas donde vamos a cargar las plantilla de navegación y el contenido de la página.

<div flex layout="row" layout-align="center" ui-view="">
 25       <md-progress-circular md-mode="indeterminate" md-diameter="96"></md-progress-circular>
 26     </div>

También agregamos al index el js para localizar el app y el formato del
API para 'restmod' (es un fix porque bower no lo agrega solo)

		$ cp bower_components/angular-restmod/dist/styles/ams.min.js app/scripts/
		$ cp angular-locale_es-co.js app/scripts/i18n/

    <script src="scripts/i18n/angular-locale_es-co.js"></script>
    <script src="scripts/ams.min.js"></script>

Luego agregamos las configuraciones de restmod, ng-token-auth y un
estado 'inicio' de prueba.

		var app = angular.module('app', [
		    'ngCookies',
		    'ngMessages',
		    'ngSanitize',
		    'ui.router',
		    'restmod',
		    'ng-token-auth',
		    'ngMaterial',
		  ]);

// Constantes
 21 app.constant('Options', {
 22   baseUrl: 'http://localhost:9000',
 23   //apiUrl : 'http://localhost:3000',
 24   apiUrl : 'https://gap-api.herokuapp.com',
 25 });
 26 
 27 // Restmod
 28 app.config(function(restmodProvider) {
 29   restmodProvider.rebase('AMSApi')
 30 })
 31 
 32 // ng-token-auth
 33 app.config(function($authProvider) {
 34   $authProvider.configure({
 35     apiUrl: 'https://gap-api.herokuapp.com'
 36   })
 37 })
 38 
 39 // Material Design
 40 .config(function($mdThemingProvider) {
 41   $mdThemingProvider.theme('default')
 42     .primaryPalette('grey', {
 43       'default': '600',
 44       'hue-1': '900'
 45     })
 46     .accentPalette('green', {
 47       'default': '800'
 48     })
 49 });

touch app/scripts/estados/inicio.js (se agrega al index)

app.config(function($stateProvider, $urlRouterProvider) {
  2   $stateProvider
  
  4   .state('inicio', {                                                       
  5     url: '/',                                                              
  6     templateUrl: 'views/inicio.html',                                      
  7     controller: ['$scope',                                                 
  8       function($scope) {                                                   
  9         $scope.sikkerdata = 'Sikkerdata';                                  
 10       }                                                                    
 11     ]                                                                      
 12   });                                                                      
 13 }); 

touch app/views/inicio.html

# Producción

Editamos Gruntfile

		ngtemplates: {
	  	dist: {
	  	  options: {                              
					module: 'app',
	  			htmlmin: '<%= htmlmin.dist.options %>',
	  			usemin: 'scripts/scripts.js'
	  		},                                                                    
	  		cwd: '<%= yeoman.app %>',                               
				src: 'views/**/*.html',
	  		dest: '.tmp/templateCache.js'
	  	}
	  },

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

Quitamos 'dist/' de .gitignore
Luego probamos con foreman y hacemos el commit.

		$ grunt build
		$ foreman start
		$ git add -A
		$ git commit -m "Configura producción en Heroku"
		
Iniciamos sesión en heroku, creamos el app y luego subimos el repositorio.Heroku
		
		$ heroku login
    $ heroku create webapp
    $ git push --set-upstream heroku master

## Íconos
$ bower install angular-material-icons --save
https://klarsys.github.io/angular-material-icons/#















## Material Design

https://github.com/angular/material-start/tree/es5-tutorial

## Estados

Crear enlace al estado

		<li ui-sref-active="active"><a ui-sref="sadmin.vehiculos">Vehículos</a></li>

Iniciar el estado
	
		$ cp app/scripts/estados/sadmin/empresas.js app/scripts/estados/sadmin/Untitled documentvehiculos.js

Editar estado

		$ vim app/scripts/estados/sadmin/vehiculos.js 	# ctr + n until no more matches and all ok
		:%s/Empresas/Vehículos/g
		:%s/empresas/vehiculos/g
		:%s/¡Empresa/¡Vehículo/g
		:%s/Empresa/Vehiculo/g
		:%s/vehiculosFiltradas/vehiculosFiltrados/g
		:%s/empresaId/vehiculoId/g
		:%s/empresa/vehiculo/g
		:%s/la vehiculo/el vehículo/g
		:%s/nueva/nuevo/g
		:%s/creada/creado/g
		:%s/nuevaVehiculo/nuevoVehiculo/g

Agregar estado al index

		<script src="scripts/estados/sadmin/vehiculos.js"></script>

Cargar vistas

		$ cp -r app/views/sadmin/empresas app/views/sadmin/vehiculos

Editar vista Index

		$ vim app/views/sadmin/vehiculos/index.html
		:%s/crearEmpresa/crearVehiculo/g
		:%s/verEmpresa(e.id)/verVehiculo(v.id)/g
		:%s/editarEmpresa(e.id)/editarVehiculo(v.id)/g
		:%s/eliminarEmpresa(e.id)/eliminarVehiculo(v.id)/g
		:%s/Empresa/Vehiculo/g
		:%s/lista-empresas/lista-vehiculos/g
		# Editar tabla a mano.
		# Subir lo que se necesite a Cloudinary

Crear modelo

		// Vehiculo
		app.factory('Vehiculo', function(restmod) {
		  //return restmod.model('https://taxis-api.herokuapp.com/usuarios')
			return restmod.model('http://localhost:3000/vehiculos')
		})

Tests

	* ¿Se ven bien las imágenes?
	* ¿Están bien los campos de la tabla?
	* ¿Sirven los campos de búsqueda?
	* ¿Sirve limpiar los campos de búsqueda?
	* ¿Sirve la paginación?

Editar vista Ver

		<h3 class="modal-title">Ver Vehículo</h3>

Ajustar controlador si se requiere.

Tests

	* ¿Se ve bien la información?

Heroku

		Cambiar a URLS de producción
		$ grunt build
		$ foreman start
		Volver a URLS de desarrollo
		$ git add -A
		$ git commit -m "ok"
		$ git push heroku