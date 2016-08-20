# devise_token_auth

Para implementar la funcionalidad de autenticación en el API utilizamos la gema [devise_token_auth](https://github.com/lynndylanhurley/devise_token_auth).  
Esta gema está basada en [devise](https://github.com/plataformatec/devise) y le agrega el esquema de [autenticación por tokens](https://carlosazaustre.es/blog/que-es-la-autenticacion-con-token/).  

## Instalación

Para instalar la gema, la agregamos al `Gemfile`. Esta gema requiere que
esté instalada la gema `devise` y si se va a utilizar la funcionalidad
de autenticación por medio de proveedores, también `omniauth`.  

    gem 'devise_token_auth', '0.1.31'
    gem 'devise', '3.5.1'
    gem 'omniauth', '1.2.2'

las gemas las instalamos con `bundle`

    $ bundle install

## Configuración Modelos

Podemos crear los modelos directamente con la gema, o modificar modelos existentes para que usen su funcionalidad.  

### Crear modelo Usuario con devise_token_auth

`devise_token_auth` nos permite generar un modelo con las características necesarias para utilizar la autenticación por tokens con el siguiente comando:  

    $ rails g devise_token_auth:install Usuario

Esto nos crea los siguientes archivos:  

  * `config/initializers/devise_token_auth.rb`
  * `db/migrate/20150812125605_devise_token_auth_create_usuarios.rb`
  * `app/models/usuario.rb`

Estos archivos son el inicializador que configura la gema `devise_token_auth`, la migración que crea la tabla Usuarios en la base de datos y la definición del modelo Usuario.  

El generador también incluye las rutas proporcionadas por `devise_token_auth` en `config/routes.rb`.  

    mount_devise_token_auth_for 'Usuario', at: '/auth'

Se debe verificar que en el controlador de la aplicación se incluyan los
métodos de `devise_token_auth`  

    class ApplicationController < ActionController::API
      include DeviseTokenAuth::Concerns::SetUserByToken
    end

En el modelo podemos ver que se incluyen los módulos de `devise` por
defecto, si queremos podemos agregar o quitar módulos según nuestras
necesidades  

    # Include default devise modules.
    devise :database_authenticatable, :registerable,
           :recoverable, :rememberable, :trackable, :validatable,
           :confirmable, :omniauthable
    include DeviseTokenAuth::Concerns::User

En la migración generada para crear la tabla Usuarios se definen los campos de la tabla, y es allí donde vamos a incluir los campos adicionales que se requieren para el usuario y quitar los que no vamos a utilizar.  

Luego de hacer las modificaciones aplicamos la migración para crear la tabla en la base de datos  

    $ rake db:migrate

## Parámetros Adicionales

Si para la creación o actualización de nuestros modelos utilizamos más parámetros que
los que utiliza devise por defecto (email, password y
password_confirmation), tenemos que decirle a devise cuáles son los
parámetros adicionales para que los pueda pasar al modelo. Esto lo
hacemos con un filtro en `app/controllers/application_controller.rb`.  

    before_action :configurar_parametros_permitidos, if: :devise_controller?

    protected

      def configurar_parametros_permitidos
        devise_parameter_sanitizer.for(:sign_up) << :nombre
        devise_parameter_sanitizer.for(:sign_up) << :imagen
        devise_parameter_sanitizer.for(:sign_up) << :celular
        devise_parameter_sanitizer.for(:account_update) << :nombre
        devise_parameter_sanitizer.for(:account_update) << :imagen
        devise_parameter_sanitizer.for(:account_update) << :celular
      end

## API devise_token_auth

Esta gema nos proporciona las siguientes rutas

|Método|Ruta|Descripción|
|:---:|:---|:---|
|POST|/auth|Crea un usuario y envía correo de confirmación. Los parámetros
por defecto son `email`, `password` y `password_confirmation`. Se pueden
agregar más parámetros usando
[`devise_parameter_sanitizer`](https://github.com/plataformatec/devise#strong-parameters)|
|DELETE|/auth|Borra el usuario identificado por el `uid` y `auth_token`|
|PUT|/auth|Actualiza una cuenta. Los parámetros por defecto son
`password` y `password_confirmation`. Se pueden agregar más parámetros
usando [`devise_parameter_sanitizer`](https://github.com/plataformatec/devise#strong-parameters). Con `config.check_current_password_before_update` se especifica si se solicita la clave actual antes de editar todos los atributos o solo la clave.|
|POST|/auth/sign_in|Autenticación con Email, acepta los parámetros
`email` y `password`. Devuelve el Usuario autenticado en JSON|
|DELETE|/auth/sign_out|Finaliza la sesión del usuario actual invalidando
el token de autenticación|
|GET|/auth/validate_token|Valida el token del usuario, acepta como
parámetros `**uid**` y `**access-token**`|
|POST|/auth/password|Envía un correo de confirmación de cambio de
contraseña. Recibe como parámetros `**email**` y `**redirect_url**`, que
es la url a la que se redirige luego de visitar el link del correo|
|PUT|/auth/password|Cambia la contraseña del usuario, acepta
`**password**` y `**password_confirmation**`. Esta ruta es válida sólo
para usuarios registrados por correo. También revisa
`**current_password**` si `config.check_current_password_before_update`
no es falso (lo es por defecto)|
|GET|/auth/password/edit|Verifica usuario utilizando
`password_reset_token`. Es el destino de la URL de los correos de
confirmación de cambio de contraseña|

### Pruebas

Creamos el archivo en el que vamos a escribir las pruebas de integración de la gema:  

    $ touch spec/requests/auth_spec.rb

Y escribimos las pruebas para las diferentes rutas

## Métodos Controladores

Esta gema permite acceder a los siguientes helpers de devise:  

|Método|Descripción|
|:----:|:----:|
|`before_action :authenticate_user!`|Devuelve un error 401 a menos que el usuario esté logueado|
|`current_user`|Devuelve el usuario actualmente logueado o nil si no hay alguno|
|`user_signed_in?`|Devuelve verdadero si un usuario está logueado, si no falso|
|`devise_token_auth_group`|Opera sobre múltiples clases de usuario como un grupo. [Leer más](https://github.com/lynndylanhurley/devise_token_auth#group-access)|

### Ejemplo

Vamos a utilizar el método `current_user` para restringir el acceso a
todas las acciones de la aplicación a sólo los usuarios administradores.  

Agregamos el filtro `before_action :validar_admin` al controlador de la
aplicación para que tenga efecto en todos los controladores. En este
filtro verificamos que exista un usuario logueado y que además este
usuario sea un administrador, de lo contrario devolvemos un mensaje de
error con estado `401`.  

    def validar_admin
      unless current_usuario && current_usuario.es_admin?
        render json: { errors: "Acceso restringido. Solo Administradores"  }, status: :unauthorized
      end
    end

En este caso se utiliza para la autenticación el modelo Usuario, por lo
que el método generado por devise_token_auth es `current_usuario`.  

# Más info

  * [Devise Strong Parameters](https://github.com/plataformatec/devise#strong-parameters)
