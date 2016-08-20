# Contenido

  * [Comandos útiles](https://github.com/vecinorg/docs/wiki/Ionic-comandos-utiles)
  * [Menú Lateral](https://github.com/vecinorg/docs/wiki/Menu-Lateral-Ionic)
  * [Autenticación](https://github.com/vecinorg/docs/wiki/Autenticacion-Ionic)
  * [Recursos](https://github.com/vecinorg/docs/wiki/Recursos-Ionic)

# Instalación Ionic

Se debe tener instalado [node.js]() y [gulp]()

    $ npm install -g cordova ionic

## Android

Toca bajar el [sdk](), e indicar la ruta en `.bash_profile`.

Más info [acá](http://cordova.apache.org/docs/en/3.4.0/guide_platforms_android_index.md.html#Android%20Platform%20Guide)

Instalar últimas versiones del SDK

    $ android

# Iniciar Aplicación Ionic

    $ ionic start vMercadosAdmin blank --appname "Mercados Admin" --id "co.com.vecino.vmercadosadmin" --sass

## Sass

    $ ionic setup sass
    $ gulp sass

el sass se escribe en `scss/ionic.app.scss`

## Info

Se ingresa la información de las etiquetas `description` y `author` en `config.xml`  

    <description>
      Aplicación que muestra una lista de Objetos y permite ver información detallada de cada uno.
    </description>
    <author email="programador@vecino.com.co" href="http://vecino.com.co/">
      vecino
    </author>

## Estructura

Primero modificamos en el index el nombre del módulo de la aplicación
`ng-app` y el contenido de la etiqueta `<body>` para que en lugar de
mostrarnos un contenido estático, nos muestre una vista  

    <body ng-app="app">

      <ion-nav-bar class="bar-dark nav-title-slide-ios7">
        <!-- Agrega botón de regreso en caso de que esté habilitado -->
        <ion-nav-back-button class="button-clear button-icon">
          <i class="icon ion-arrow-left-c"></i>
        </ion-nav-back-button>
      </ion-nav-bar>

      <!-- Acá se van a cargar las vistas -->
      <ion-nav-view></ion-nav-view>

    </body>

Luego modificamos el nombre del módulo de la aplicación en
`www/js/app.js` para que coincida con el del index y lo asignamos
la variable `app`  

    var app = angular.module('app', ['ionic'])

A continuación creamos el estado `home` en `js/estados/app.js`  

    app.config(function($stateProvider, $urlRouterProvider) {
      $stateProvider

      // Home
      .state('home', {
        url: '/',
        templateUrl: 'vistas/app/home.html',
      })

      // Otherwise
      $urlRouterProvider.otherwise('/')
    })

Con este estado cargamos la vista `vistas/app/home.html` en el index  

    <!-- Vista -->
    <ion-view>

      <!-- Título -->
      <ion-nav-title>App</ion-nav-title>

      <!-- Contenido -->
      <ion-content class="padding">
        <h1>Home Page</h1>
      </ion-content>
      <!-- /Contenido -->

    </ion-view>
    <!-- /Vista -->

## Probar aplicación

Para probar la aplicación en el navegador  

    $ ionic serve

la aplicación corre en http://localhost:8100/ionic-lab

# Configuración ionic io

Para configurar servicios como analytics, deploy y notificaciones push.  

[Documentación](http://docs.ionic.io/docs)  

    $ ionic add ionic-platform-web-client

# Subir App a Ionic Vew

Para poder compartir la aplicación con el equipo de desarrollo

    $ ionic upload
