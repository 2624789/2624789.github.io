Para la creación de recursos en nuestra API, vamos a utilizar la
metodología **Desarrollo dirigido por Pruebas**, esta metodología
establece que para cada unidad de software, el programador debe:  

  1. Definir primero un conjunto de pruebas de la unidad
  2. Implementar la unidad
  3. Verificar que la implementación de la unidad hace que las pruebas se pasen

## Modelo

Generamos el modelo  

    $ rails g model Usuario nombre:string correo:string
reputacion:decimal

este comando nos genera los archivos del modelo, la migración, los specs
y las factories (se debe tener previamente configurado rspec como el
framework de pruebas).  

Una vez generados estos archivos, podemos editar la migración según
nuestras necesidades y luego aplicarla para poder empezar a trabajar en
el recurso.  

## Pruebas Unitarias

Las Pruebas Unitarias nos permiten comprobar el correcto funcionamiento
de un módulo de código específico por separado, en este caso el modelo
del recurso.  

Utilizamos los archivos: `spec/models/usuario_spec.rb` y
`spec/factories/usuarios.rb`.  

### Factories

Las factories se utilizan para crear objetos que se utilizaránn en las
pruebas.  

#### Ejemplo

    FactoryGirl.define do

      factory :usuario_valido, class: Usuario do
        nombre "Usuario Válido"
        correo "usuario@correo.com"
        reputacion 1000
        celular "3003453234"
        imagen "imagen.png"
      end

    end

### Estructura de las Pruebas Unitarias

Las Pruebas Unitarias para cualquier modelo deben verificar lo siguiente:  

  * Cuando se pasan atributos válidos, el objeto es válido
  * Cuando se pasan atributos no válidos, el objeto no es válido
  * Los métodos de instancia y de clase funcionan como deberían

#### Ejemplo

    require 'rails_helper'

    RSpec.describe Usuario, type: :model do

      context "Atributos Válidos" do
        it "es válido con nombre, correo y celular"
      end

      context "Atributos no Válidos" do
        it "no es válido sin nombre"
        it "no es válido sin correo"
        it "no es válido sin celular"
        it "no es válido si el correo no es único"
        it "no es válido si la reputación es negativa"
      end

    end


### Modelo

Luego de escribir las pruebas del modelo, lo más común es que ninguna de
estas pasen, entonces se deben hacer las implementaciones necesarias en
el modelo para pasarlas.  

Estas implementaciones generalmente son validaciones de atributos y 
métodos de clase o de instancia.  

### Ejemplo

    class Usuario < ActiveRecord::Base
      # Validaciones
      validates_presence_of :nombre, :email, :celular
      validates_uniqueness_of :email
      validates :reputacion,
           numericality: { greater_than_or_equal_to: 0, only_integer: true  }
    end

## Pruebas de Integración, Rutas, Controladores y Serializadores

### Pruebas

Se genera el archivo para escribir las pruebas  

    $ rails g rspec:request usuario

y se escriben para cada una de las rutas que va a exponer el recurso RESTful  

    require 'rails_helper'

    RSpec.describe "Usuarios", type: :request do

      before :each do
        @cabeceras_peticion = {
          "Accept": "application/json",
          "Content-Type": "application/json"
        }
        @usuario_uno = FactoryGirl.create :usuario_uno
        @usuario_dos = FactoryGirl.create :usuario_dos
      end

      # index
      describe "GET /usuarios" do
        it "Devuelve todos los usuarios" do
          get "/usuarios", {}, @cabeceras_peticion

          expect(response.status).to eq 200 # OK

          body = JSON.parse(response.body)
          usuarios = body['usuarios']
          nombres_usuario = usuarios.map { |m| m["nombre"] }
          emails_usuario = usuarios.map { |m| m["email"] }
          reputaciones_usuario = usuarios.map { |m| m["reputacion"] }
          celulares_usuario = usuarios.map { |m| m["celular"] }
          imagenes_usuario = usuarios.map { |m| m["imagen"] }

          expect(nombres_usuario).to match_array([@usuario_uno.nombre, @usuario_dos.nombre])
          expect(emails_usuario).to match_array([@usuario_uno.email, @usuario_dos.email])
          expect(reputaciones_usuario).to match_array([@usuario_uno.reputacion, @usuario_dos.reputacion])
          expect(celulares_usuario).to match_array([@usuario_uno.celular, @usuario_dos.celular])
          expect(imagenes_usuario).to match_array([@usuario_uno.imagen, @usuario_dos.imagen])
        end
      end

      # show
      describe "GET /usuarios/:id" do
        it "Devuelve el usuario solicitado" do
          get "/usuarios/#{@usuario_uno.id}", {}, @cabeceras_peticion
          expect(response.status).to be 200 # OK

          body = JSON.parse(response.body)
          usuario = body['usuario']

          expect(usuario["email"]).to eq @usuario_uno.email
          expect(usuario["nombre"]).to eq @usuario_uno.nombre
          expect(usuario["reputacion"]).to eq @usuario_uno.reputacion
          expect(usuario["celular"]).to eq @usuario_uno.celular
          expect(usuario["imagen"]).to eq @usuario_uno.imagen
        end
      end

      # destroy
      describe "DELETE /usuarios/:id" do
        it "Elimina un usuario" do
          delete "/usuarios/#{@usuario_uno.id}", {}, @cabeceras_peticion

          expect(response.status).to be 204 # No Content
          expect(Usuario.exists?(@usuario_uno.id)).to eq false
        end
      end

      # create
      describe "POST /usuarios" do
        it "Crea un usuario" do
          parametros_usuario = FactoryGirl.attributes_for(:usuario_valido).to_json
          usuario_valido = FactoryGirl.build :usuario_valido

          post "/usuarios", parametros_usuario, @cabeceras_peticion

          expect(response.status).to eq 201 # Created
          expect(Usuario.last.email).to eq usuario_valido.email
          expect(Usuario.last.nombre).to eq usuario_valido.nombre
          expect(Usuario.last.imagen).to eq usuario_valido.imagen
          expect(Usuario.last.celular).to eq usuario_valido.celular
        end
      end

      # update
      describe "PUT /usuarios/:id" do
        it "Actualiza un usuario" do
          parametros_usuario = FactoryGirl.attributes_for(:usuario_valido).to_json
          usuario_valido = FactoryGirl.build :usuario_valido

          put "/usuarios/#{@usuario_uno.id}", parametros_usuario, @cabeceras_peticion

          expect(response.status).to be 204 # No content
          expect(Usuario.find(@usuario_uno.id).email).to eq usuario_valido.email
          expect(Usuario.find(@usuario_uno.id).nombre).to eq usuario_valido.nombre
          expect(Usuario.find(@usuario_uno.id).imagen).to eq usuario_valido.imagen
          expect(Usuario.find(@usuario_uno.id).celular).to eq usuario_valido.celular
          # Este atributo no se puede editar
          expect(Usuario.find(@usuario_uno.id).reputacion).to eq @usuario_uno.reputacion
        end
      end
    end

### Rutas

Se agregan las rutas

    resources :usuarios,
      except: [:edit, :new],
      defaults: { format: :json }

### Controlador

Se genera el controlador

    $ rails g controller usuarios --no-controller-specs

### Serializador

Se genera el serializador

    $ rails g serializer Usuario
