## RED

Siguiendo un enfoque [TDD](), se empieza por escribir los tests de
integración que debe pasar el API.

    $ rails generate rspec:request transaccion

En un API los tests de integración son del tipo `request` porque el
sistema sólo recibirá peticiones a través de sus URLs y responderá con
información en formato [JSON](), así que al simular una petición o
request, y verificar que su respuesta sea correcta, estamos evaluando
todos los componentes del API.

Los tests deben evaluar cada una de las peticiones que puede atender el
API en cada uno de los contextos en que se utilice el sistema:

    RSpec.describe "Transacciones API", type: :request do

      # index
      describe "GET /transacciones" do
        context "usuario no autenticado" do
          it "No permite la acción"
        end
      end

    end

Primero escribimos el título de cada uno de los tests, las sentencias `it`.
Luego de tener la lista completa y revisada, escribimos los tests y
verificamos que no pase ninguno.

## GREEN

El paso siguiente es escribir el código que se requiera en el API para
pasar los tests definidos anteriormente. En un API en Rails generalmente
se requiere implementar los siguientes componentes:

  * **rutas**: Las rutas son las interfaces que le permiten al API
    recibir peticiones HTTP enviadas desde los diferentes clientes.

    resources :vecinos,
      except: [:edit, :new],
      defaults: { format: :json } 

  * **controladores**: Los controladores son las piezas de código que se
    encargan de interpretar las peticiones enviadas por el cliente y
    realizar las operaciones propias del negocio para dar una respuesta
    adecuada.

    $ rails g scaffold_controller Vecinos --no-controller-specs --no-routing-specs --no-request-specs

  * **serializadores**: Los serializadores son las interfaces a través
    de las cuales el API entrega una respuesta a cada solicitud enviada
    desde los clientes.

## CanCanCan

Gemfile:

    gem 'cancancan', '1.13.1'

    $ bundle install

    class ApplicationController < ActionController::API
      include CanCan::ControllerAdditions

      rescue_from CanCan::AccessDenied do |exception|
        render json: { errors: "Acceso restringido"  }, status: :forbidden
      end

      protected
   
        def current_ability
          @current_ability ||= Ability.new(current_vecino)
        end
    end

Ability
    
    class Ability
      include CanCan::Ability
      
      def initialize(vecino)
        vecino ||= Vecino.new email: nil
        can :read, Vecino, Vecino.con_negocio do |vecino|
          vecino.nombre_negocio.not_blank?
        end
      end
    end

Modelo

    scope :con_negocio, -> { where.not(nombre_negocio: nil) }

Controlador:

    class VecinosController < ApplicationController
      load_and_authorize_resource

      # GET /vecinos
      def index
        render json: @vecinos
      end

      # GET /vecinos/1
      def show
        render json: @vecino
      end

      # POST /vecinos
      def create
        if @vecino.save
          render json: @vecino, status: :created, location: @vecino
        else
          render json: @vecino.errors, status: :unprocessable_entity
        end
      end

      # PATCH/PUT /vecinos/1
      def update
        if @vecino.update(vecino_params)
          head :no_content
        else
          render json: @vecino.errors, status: :unprocessable_entity
        end
      end

      # DELETE /vecinos/1
      def destroy
        @vecino.destroy
        head :no_content
      end

      private

        def vecino_params
          params[:vecino]
        end
    end

Serializador

    gem 'active_model_serializers', '0.8.3'

    $ bundle install
    $ rails g serializer Vecino

  class VecinoSerializer < ActiveModel::Serializer
    attributes :id,
      :nombre_negocio,
      :direccion_negocio,
      :imagen_negocio,
      :latitud_negocio,
      :longitud_negocio
  end 