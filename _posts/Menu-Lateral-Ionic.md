# Estados

Para usar un menú lateral tenemos que definir un estado **abstracto** `menu`
en el cual cargar la plantilla con el menú y una vista para mostrar el
contenido de los subestados que utilicen un menú lateral.  

Este estado lo creamos en los estados de la aplicación
`js/estados/app.js`.  

En este caso también vamos a crear el estado hijo `menu.home`, que
corresponde al estado hijo que utilizará el menú lateral.  

    app.config(function($stateProvider, $urlRouterProvider) {
      $stateProvider

      // Menú Lateral
      .state('menu', {
        abstract: true,
        templateUrl: 'vistas/app/menu.html',

      })

      // Home
      .state('menu.home', {
        url: '/',
        templateUrl: 'vistas/app/home.html',
      })

      // Otherwise
      $urlRouterProvider.otherwise('/')

    })

# Vistas

La vista del menú define dos elementos principales, `<ion-side-menu>` que es
el menú que se muestra al deslizar hacia un lado la pantalla o al tocar
el botón del menú, y `<ion-side-menu-content>` que es el contenido de que se muestra cuando el
menú no está desplegado, en este caso, el botón de menú y la vista del estado hijo.  

    <ion-side-menus>

      <ion-side-menu side="left">
        <header class="bar bar-header bar-dark">
          <h1 class="title">Menú</h1>
        </header>
        <ion-content class="has-header">

          <img src="img/app.jpg" width="100%" height="100px"/>

          <ion-list>

            <ion-item class="item-icon-left" cerrar-menu ui-sref="nuevaSesion">
              <i class="icon ion-log-in"></i>
              Iniciar Sesión
            </ion-item>

            <ion-item class="item-icon-left" cerrar-menu ui-sref="nuevoUsuario">
              <i class="icon ion-person-add"></i>
              Registrarse
            </ion-item>

          </ion-list>

        </ion-content>
      </ion-side-menu>

      <ion-side-menu-content>
        <ion-nav-bar class="bar-dark nav-title-slide-ios7">
          <ion-nav-buttons side="left">
            <button class="button button-icon button-clear ion-navicon"
              ng-click="toggleLeft()">
            </button>
          </ion-nav-buttons>
        </ion-nav-bar>
        <!-- Vista -->
        <ion-nav-view></ion-nav-view>
      </ion-side-menu-content>

    </ion-side-menus>

## Cerrar Menú

La directiva `menu-close` utilizada por defecto por Ionic para cerrar el
menú lateral cuando se escoge una opción elimina el historial de
navegación por lo que luego no podremos volver atrás.  

Para solucionar este problema escribimos en `www/js/directivas.js` la
directiva `cerrar-menu`, la cual tiene la misma funcionalidad que
`menu-close` pero no borra el historial de navegación.  

    app.directive('cerrarMenu', ['$ionicHistory', function($ionicHistory) {
      return {
        restrict: 'AC',
        link: function($scope, $element) {
          $element.bind('click', function() {
            var sideMenuCtrl = $element.inheritedData('$ionSideMenusController');
            if (sideMenuCtrl) {
              $ionicHistory.nextViewOptions({
                historyRoot: false,
                disableAnimate: true,
                expire: 300
              });
              sideMenuCtrl.close();
            }
          });
        }
      };
    }]);

## Desplegar Menú

Para desplegar el menú utilizando el botón, tenemos que agregar la
función `toggleLeft` a las funciones globales de la aplicación en
`js/app.js`  

    // Funciones Globales
    .run(function($rootScope, $ionicSideMenuDelegate) {

      // Desplegar Menú Lateral
      $rootScope.toggleLeft = function() {
        $ionicSideMenuDelegate.toggleLeft()
      }

    })

# Más info

  * [ion-side-menus](http://ionicframework.com/docs/api/directive/ionSideMenus/)
  * [ionic-starter-sidemenu](https://github.com/driftyco/ionic-starter-sidemenu)
  * [ionic history](http://ionicframework.com/docs/api/service/$ionicHistory/)
  * [menu-close](http://ionicframework.com/docs/api/directive/menuClose/)
  * [history stack using side menu](http://stackoverflow.com/questions/28646894/ionic-history-stack-when-using-side-menu-and-tabs)
