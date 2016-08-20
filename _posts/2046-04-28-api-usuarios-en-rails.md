Crear Rama

		$ git checkout -b vecinos

# devise_token_auth

Para implementar la funcionalidad de autenticación en el API utilizamos la gema [devise_token_auth](https://github.com/lynndylanhurley/devise_token_auth).  

Esta gema está basada en [devise](https://github.com/plataformatec/devise) y le agrega el esquema de [autenticación por tokens](https://carlosazaustre.es/blog/que-es-la-autenticacion-con-token/).  

## Instalación

Para instalar la gema, la agregamos al `Gemfile`. Esta gema requiere que
esté instalada la gema `devise`.

    gem 'devise_token_auth', '0.1.31'
    gem 'devise', '3.5.1'
    gem 'omniauth', '1.2.2'

las gemas las instalamos con `bundle`

    $ bundle install

## Crear modelo Usuario con devise_token_auth

`devise_token_auth` nos permite generar un modelo con las características necesarias para utilizar la autenticación por tokens con el siguiente comando:  

    $ rails g devise_token_auth:install Vecino

Esto nos crea los siguientes archivos:  

  * `config/initializers/devise_token_auth.rb`
  * `db/migrate/20150812125605_devise_token_auth_create_usuarios.rb`
  * `app/models/usuario.rb`

Estos archivos son el inicializador que configura la gema `devise_token_auth`, la migración que crea la tabla Usuarios en la base de datos y la definición del modelo Usuario.  

En el modelo podemos ver que se incluyen los módulos de `devise` por
defecto, si queremos podemos agregar o quitar módulos según nuestras
necesidades  

    # Include default devise modules.
    devise :database_authenticatable, :registerable,
           :recoverable, :rememberable, :trackable, :validatable,
           :confirmable, :omniauthable
    include DeviseTokenAuth::Concerns::User

## Rutas

El generador también incluye las rutas proporcionadas por `devise_token_auth` en `config/routes.rb`.  

    mount_devise_token_auth_for 'Vecino', at: '/auth'

## Métodos

Se debe verificar que en el controlador de la aplicación se incluyan los
métodos de `devise_token_auth`  

    class ApplicationController < ActionController::API
      include DeviseTokenAuth::Concerns::SetUserByToken
    end

Esta gema permite acceder a los siguientes helpers de devise:  

|Método|Descripción|
|:----:|:----:|
|`before_action :authenticate_vecino!`|Devuelve un error 401 a menos que el usuario esté logueado|
|`current_vecino`|Devuelve el usuario actualmente logueado o nil si no hay alguno|
|`vecino_signed_in?`|Devuelve verdadero si un usuario está logueado, si no falso|
|`devise_token_auth_group`|Opera sobre múltiples clases de usuario como un grupo. [Leer más](https://github.com/lynndylanhurley/devise_token_auth#group-access)|

## Migración

En la migración generada para crear la tabla Usuarios se definen los campos de la tabla, y es allí donde vamos a incluir los campos adicionales que se requieren para el usuario y quitar los que no vamos a utilizar.  

		## User Info
		t.string :nombre, limit: 50, null: false
		t.string :imagen, limit: 50
		t.string :email, null: false
		t.string :celular, limit: 15
		t.boolean :admin, default: false
		t.string :nombre_negocio, limit: 50
		t.string :direccion_negocio, limit: 100
		t.string :imagen_negocio, limit: 50
		t.float :longitud_negocio
		t.float :latitud_negocio

Luego de hacer las modificaciones aplicamos la migración para crear la tabla en la base de datos  

    $ rake db:migrate RAILS_ENV=test
    $ rake db:migrate

Si faltó algo por agregar, y se necesita corregir la migración, se hace `rollback`, se modifica la migración y luego se aplica de nuevo.

    $ rake db:rollback
    $ rake db:rollback RAILS_ENV=test

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

## Pruebas Unitarias

		$ rails g rspec:model Vecino

Escribimos un listado de las pruebas unitarias del modelo, es decir, las
condiciones que deben cumplir los atributos de los objetos creados a
partir del modelo.

    context "Atributos válidos" do
      it "es válido con todos sus atributos"
    end

    context "Atributos requeridos" do
  		it "No es válido sin placa"
    end

    context "Atributos no válidos" do
      it "No es válido si placa no tiene 6 caracteres"
      it "No es válido si marca tiene más de 50 caracteres"
      it "No es válido si modelo tiene más de 50 caracteres"

      it "No es válido si kilometraje es menor que cero"

      it "No es válido si placa no es válida"

      it "No es válido si kilometraje actual es menor a kilometraje inicial"
    end

    context "Métodos" do
      it "Guarda las placas en mayúsculas"
    end

Implementamos cada una de las pruebas.

    before :each do
      @vehiculo = Fabricate.build :vehiculo
    end

    context "Atributos válidos" do
      it "es válido con todos sus atributos" do
        expect(@vehiculo).to be_valid
      end
    end

Para la implementación de las tests, utilizamos datos de pruebas generados con las gemas [Fabrication](http://www.fabricationgem.org/) y [FFaker](https://github.com/ffaker/ffaker).

    fabricator :vehiculo do
      placa { FFaker::Product.letters(3) + rand.to_s[2..4] }
      marca { FFaker::Vehicle.make }
      modelo { FFaker::Vehicle.year }
    end

Una vez escritas las pruebas, se debe agregar la información específica del
modelo que se necesite para pasar las pruebas definidas.

    it "No es válido sin placa" do
      @vehiculo.placa = nil
      expect(@vehiculo).not_to be_valid
      expect(@vehiculo.errors[:placa]).to include("can't be blank")
    end

en `app/models/vehiculo.rb`:

    validates_presence_of :placa