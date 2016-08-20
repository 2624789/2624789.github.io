The frontend uses AngularJS and Gulp for compiling the Less sources. The backend is a Nodejs RESTful API associated to a MongoDB instance. registration free

# Rutas y Estados

ui-router .The states are kind of simple. We need a default state home mapped to the route /home displaying a landing page to new users. And we need an abstract state sheets (/sheets/:id) which is the root state of a shared sheet. This state has sub-states like sheets.expenses, sheets.friends and sheets.overview automatically mapped to /sheets/:id/expenses, /sheets/:id/friends and /sheets/:id/

		$ touch app/scripts/rutas.js

Se agrega al index

		<script src="scripts/rutas.js"></script>

Se escriben las rutas y los estados:

		

		app.config(function($stateProvider, $urlRouterProvider) {
		    $urlRouterProvider
		    	.otherwise('/inicio');
		 
		    $stateProvider
		      .state('inicio', {	// Landing Page
		        url: '/inicio',
		        templateUrl: 'vistas/inicio.html',
		        controller: 'InicioCtrl'
		      })
	        .state('cuentas', {		// Estado Abstracto (Se trae la Colección)
            abstract: true,
            url: '/cuentas',
            template: '<ui-view></ui-view>',
            controller: 'CuentasCtrl'
        	})
	      		.state('cuentas.index', {
	      			url: '/',
	      			templateUrl: 'vistas/cuentas.index.html'
	      		})
	          .state('cuentas.ver', {
	            url: '/:id',
	            templateUrl: 'vistas/cuentas.ver.html'
	          })
	          .state('cuentas.nueva', {
	            url: '/nueva',
	           templateUrl: 'vistas/cuentas.nueva.html'
	          })
	          .state('cuentas.editar', {
	              url: '/:id/editar',
	              templateUrl: 'vistas/cuentas.editar.html'
	          })
		});

