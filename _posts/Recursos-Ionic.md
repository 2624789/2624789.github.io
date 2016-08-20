Para la gestión de recursos en Ionic tenemos que crear los diferentes
estados que nos permitirán efectuar operaciones CRUD.   

# Interfaz de Datos

## Restmod

Lo primero que hacemos es configurar una interfaz de datos para poder
interactuar de forma transparente con el API.  

Para la interfaz de datos vamos a utilizar el módulo
[Restmond](https://github.com/platanus/angular-restmod)  

Lo instalamos con `bower`  

    $ bower install angular-restmod --save

lo agregamos al index  

    <!-- restmod -->
    <script src="lib/angular-restmod/dist/angular-restmod-bundle.min.js"></script>
    <script src="lib/angular-restmod/dist/styles/ams.min.js"></script>

y a la aplicaicón Ionic  

    var app = angular.module('app', ['ionic', 'restmod'])

Para configurarlo tenemos que establecer el estilo del API  

    // Restmod
    app.config(function(restmodProvider) {
      restmodProvider.rebase('AMSApi')
    })

## Modelos

Una vez instalado y configurado Restmod, podemos crear nuestros modelos
para interactuar con el API de forma transparente.  

Creamos el arhivo `www/js/modelos.js` y lo agregamos al index. En este
archivo escribimos el modelo del recurso  

    // Producto
    app.factory('Producto', function(restmod) {
      return restmod.model('https://api-busumer.herokuapp.com/productos')
    })

# Estados

Una vez establecida la interfaz de datos, utilizamos el modelo para
traer los recursos desde el API y utilizarlos en los controladores.  

    // Lista de Productos
    .state('productos', {
      url: '/productos',
      templateUrl: 'vistas/productos/productos.html',
      resolve: {
        productos: ['Producto',
          function(Producto) {
            return Producto.$search().$asPromise()
          }],
      },
      controller: ['$scope', 'productos',
        function($scope, productos) {
          $scope.productos = productos
        }]
    })

# Vistas

Y creamos las vistas necesarias para interactuar con los recursos.  

