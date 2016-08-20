# Angular Token Auth

Para implementar la autenticación utilizamos el módulo para angular
[ng-token-auth](https://github.com/lynndylanhurley/ng-token-auth).
Este es un módulo que implementa [autenticación basada en
tokens](http://stackoverflow.com/questions/1592534/what-is-token-based-authentication) y
que funciona de forma transparente con [Devise Token
Auth](https://github.com/lynndylanhurley/devise_token_auth).  

## Instalación

Instalamos el módulo usando `bower`  

    $ bower install ng-token-auth --save

Agregamos el módulo a las dependencias de la aplicación en
`www/js/app.js`  

    var app = angular.module('app', ['ionic', 'ng-token-auth'])

Tenemos que asegurarnos que se agreguen las librerías necesarias al
index.  

    <!-- ng-token-auth -->
    <script src="lib/angular-cookie/angular-cookie.min.js"></script>
    <script src="lib/ng-token-auth/dist/ng-token-auth.min.js"></script>

## Configuración

En `www/js/app.js` agregamos la configuración necesaria para el
funcionamiento de `ng-token-auth`, que este caso es el `apiUrl` y
`storage`.  

    /// ng-token-auth
    .config(function($authProvider) {
      $authProvider.configure({
        apiUrl: 'https://api-busumer.herokuapp.com',
        storage: 'localStorage'
      })
    })

# Estados

Ahora creamos los estados que vamos a utilizar  para
implementar las funcionalidades de registro e inicio de sesión.  

## Registro

Creamos el estado `nuevoUsuario` en `www/js/estados/app.js`.  

    // Registro
    .state('nuevoUsuario', {
      url: '/usuarios/nuevo',
      templateUrl: 'vistas/app/registro.html',
      controller: ['$scope', '$auth', '$state', '$ionicPopup',
        function($scope, $auth, $state, $ionicPopup){

          // Habilita el botón para regresar al estado anterior
          $scope.$on('$ionicView.beforeEnter', function (event, viewData) {
            viewData.enableBack = true;
          })

          $scope.u = {}

          $scope.nuevoUsuario = function(usuario) {
            $auth.submitRegistration(usuario)
            .then(function(resp) {
              $auth.submitLogin({
                email: usuario.email,
                password: usuario.password
              })
              .then(function(resp) {
                $state.go('menu.home')
              })
              .catch(function(resp) {
                $ionicPopup.alert({
                  title: '¡Se creó el usuario pero no se pudo iniciar sesión!',
                  template: "Errores: " + JSON.stringify(resp.data.errors)
                })
                .then(function(resp) {
                  $state.go('menu.home')
                })
              })
            })
            .catch(function(resp) {
              $ionicPopup.alert({
                title: '¡Falló el Registro!',
                template: "Errores: " + JSON.stringify(resp.data.errors)
              })
            })
          }

        }]
    })

y la vista correspondiente en `www/vistas/app/registro.html`  


    <ion-view cache-view="false">

      <ion-nav-title>Registro</ion-nav-title>

      <ion-content class="padding">

        <form name="form" ng-submit="nuevoUsuario(u)" role="form" novalidate>

          <div class="list">

            <!-- Nombre -->
            <label for="nombre" class="item item-input item-floating-label">
              <span class="input-label">Nombre</span>
              <input type="text" id="nombre" name="uNombre" ng-model="u.nombre" class="form-control" placeholder="Nombre" required>
              <div class="mensaje-error"  ng-show="form.$submitted || form.uNombre.$touched">
                <p ng-show="form.uNombre.$error.required">Por favor ingrese su nombre</p>
              </div>
            </label>
            <!-- /Nombre -->

            <!-- Correo -->
            <label for="email" class="item item-input item-floating-label">
              <span class="input-label">Correo</span>
              <input type="email" id="email" name="uEmail" ng-model="u.email" class="form-control" placeholder="Correo" ng-pattern="/^[-a-z0-9~!$%^&*_=+}{\'?]+(\.[-a-z0-9~!$%^&*_=+}{\'?]+)*@([a-z0-9_][-a-z0-9_]*(\.[-a-z0-9_]+)*\.(aero|arpa|biz|com|coop|edu|gov|info|int|mil|museum|name|net|org|pro|travel|mobi|[a-z][a-z])|([0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}))(:[0-9]{1,5})?$/i" required>
              <div class="mensaje-error"  ng-show="form.$submitted || form.uEmail.$touched">
                <p ng-show="form.uEmail.$error.required">Por favor ingrese su correo electrónico</p>
                <p ng-show="form.uEmail.$error.pattern">Por favor ingrese un correo electrónico válido</p>
              </div>
            </label>
            <!-- /Correo -->

            <!-- Celular -->
            <label for="celular" class="item item-input item-floating-label">
              <span class="input-label">Celular</span>
              <input type="tel" id="celular" name="uCelular" ng-model="u.celular" class="form-control" placeholder="Celular" ng-pattern="/^3[0-9]{9}$/" required>
              <div class="mensaje-error"  ng-show="form.$submitted || form.uCelular.$touched">
                <p ng-show="form.uCelular.$error.required">Por favor ingrese su número de celular</p>
                <p ng-show="form.uCelular.$error.pattern">Por favor ingrese un número de celular válido</p>
              </div>
            </label>
            <!-- /Celular -->

            <!-- Password -->
            <label for="password" class="item item-input item-floating-label">
              <span class="input-label">Contraseña</span>
              <input type="password" id="password" name="uPassword" ng-model="u.password" class="form-control" placeholder="Contraseña" required ng-minlength="6">
              <div class="mensaje-error"  ng-show="form.$submitted || form.uPassword.$touched">
                <p ng-show="form.uPassword.$error.required">Por favor ingrese una contraseña</p>
                <p ng-show="form.uPassword.$error.minlength">La contraseña debe tener más de 6 caracteres</p>
              </div>
            </label>
            <!-- /Password -->

            <!-- Password Confirmation -->
            <label for="password_confirmation" class="item item-input item-floating-label">
              <span class="input-label">Confirmación contraseña</span>
              <input type="password" id="password_confirmation" name="uPasswordConfirmation" ng-model="u.password_confirmation" class="form-control" placeholder="Confirmación contraseña" required comparar-con="u.password">
              <div class="mensaje-error"  ng-show="form.$submitted || form.uPasswordConfirmation.$touched">
                <p ng-show="form.uPasswordConfirmation.$error.required">Por favor ingrese la confirmación de la contraseña</p>
                <p ng-show="form.uPasswordConfirmation.$error.compararCon">Las contraseñas deben coincidir</p>
              </div>
            </label>
            <!-- /Password Confirmation -->

          </div>

          <!-- Botón -->
          <button ng-show="form.$valid" type="submit" class="button button-full button-positive">
            Registrarse
          </button>
          <!-- /Botón -->

        </form>

      </ion-content>

    </ion-view>

En esta vista agregamos la opción `cache-view="false"` para poder
limpiar los formularios, de lo contrario la info del formulario se
guarda en el cache.  

Para validar el campo confirmación contraseña creamos la directiva
`comparar-con` en  `www/js/directivas.js`  

    // Validar que un campo de un formulario es igual a otro
    app.directive('compararCon', function() {
      return {
        require: "ngModel",
        scope: {
          valorOtroModelo: "=compararCon"
        },
        link: function(scope, element, attributes, ngModel) {
          ngModel.$validators.compararCon = function(modelValue) {
            return modelValue == scope.valorOtroModelo
          }
          scope.$watch("valorOtroModelo", function() {
            ngModel.$validate()
          })
        }
      }
    })

## Usuario Actual

Cuando se loguea un usuario, lo cargamos en `$scope.usuarioActual`
utilizando los eventos que provee `ng-token-auth`  

    $rootScope.$on('auth:login-success', function(ev, usuario) {
      $scope.usuarioActual = usuario
    })

En las vistas podemos modificar el contenido teniendo en cuenta la
existencia de `usuarioActual.id`  

    <ion-item ng-show="usuarioActual.id" class="item-icon-left" cerrar-menu ng-click="cerrarSesion()">
      <i class="icon ion-log-out"></i>
      Cerrar Sesión
    </ion-item>

    <ion-item ng-show="!usuarioActual.id" class="item-icon-left" cerrar-menu ui-sref="nuevaSesion">
      <i class="icon ion-log-in"></i>
      Iniciar Sesión
    </ion-item>

## Iniciar Sesión

Creamos el estado `nuevaSesion` en `www/js/estados/app.js`.  

    .state('nuevaSesion', {
      url: '/login',
      templateUrl: 'vistas/app/login.html',
      controller: ['$scope', '$auth', '$state', '$ionicPopup',
        function($scope, $auth, $state, $ionicPopup) {

          // Habilita el botón para regresar al estado anterior
          $scope.$on('$ionicView.beforeEnter', function (event, viewData) {
            viewData.enableBack = true;
          })

          $scope.logIn = function(usuario) {
            $auth.submitLogin(usuario)
            .then(function(resp) {
              $state.go('menu.home')
            })
            .catch(function(resp) {
              $ionicPopup.alert({
                title: '¡No se pudo iniciar sesión!',
                template: "Errores: " + JSON.stringify(resp.errors)
              })
            })
          }
        }]
    })

y la vista correspondiente en `www/vistas/app/login.html`  

    <ion-view cache-view="false">

      <ion-nav-title>Iniciar Sesión</ion-nav-title>

      <ion-content class="padding">

        <form id="form" name="form" ng-submit="logIn(u)" role="form" novalidate>

          <div class="list">

            <!-- Correo -->
            <label for="email" class="item item-input item-floating-label">
              <span class="input-label">Correo</span>
              <input type="email" id="email" name="uEmail" ng-model="u.email" class="form-control" placeholder="Correo" ng-pattern="/^[-a-z0-9~!$%^&*_=+}{\'?]+(\.[-a-z0-9~!$%^&*_=+}{\'?]+)*@([a-z0-9_][-a-z0-9_]*(\.[-a-z0-9_]+)*\.(aero|arpa|biz|com|coop|edu|gov|info|int|mil|museum|name|net|org|pro|travel|mobi|[a-z][a-z])|([0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}))(:[0-9]{1,5})?$/i" required>
              <div class="mensaje-error"  ng-show="form.$submitted || form.uEmail.$touched">
                <p ng-show="form.uEmail.$error.required">Por favor ingrese su correo electrónico</p>
                <p ng-show="form.uEmail.$error.pattern">Por favor ingrese un correo electrónico válido</p>
              </div>
            </label>
            <!-- /Correo -->

            <!-- Password -->
            <label for="password" class="item item-input item-floating-label">
              <span class="input-label">Contraseña</span>
              <input type="password" id="password" name="uPassword" ng-model="u.password" class="form-control" placeholder="Contraseña" required ng-minlength="6">
              <div class="mensaje-error"  ng-show="form.$submitted || form.uPassword.$touched">
                <p ng-show="form.uPassword.$error.required">Por favor ingrese una contraseña</p>
                <p ng-show="form.uPassword.$error.minlength">La contraseña debe tener más de 6 caracteres</p>
              </div>
            </label>
            <!-- /Password -->

          </div>

          <!-- Botón -->
          <button ng-show="form.$valid" type="submit" class="button button-full button-positive">
            Iniciar Sesión
          </button>
          <!-- /Botón -->

        </form>

      </ion-content>

    </ion-view>

## Cerrar Sesión

Para cerrar sesión creamos la función `cerrarSesion()` en el controlador
del menú lateral, que es donde estará la opción para cerrar sesión.  

    $scope.cerrarSesion = function() {
      $auth.signOut()
        .then(function(resp) {
          $scope.usuarioActual = {}
          $state.go('menu.home')
        })
    }

En esta función actualizamos `$scope.usuarioActual`, y podemos hacerlo
en otros controladores utilizando el evento `$rootScope.$on('auth:logout-success', function(ev) {})`  
