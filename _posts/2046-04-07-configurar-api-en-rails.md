# Creación del API

Instalar gema 

    $ gem install rails-api

Para crear el API vamos a utilizar la gema 'rails-api'  

    $ rails-api new api-parkout -T --database=postgresql
    $ cd api-parkout/
    $ mv README.rdoc README.md

# Git

Luego de crear el API iniciamos un repositorio Git

    $ git init
    $ git config user.name "lenieto3"
    $ git config user.email "lenieto3@machete.com.co"
    $ git add -A
    $ git commit -m "Comienzo"

Después creamos un repositorio remoto en Bitbucket y lo asociamos al repositorio local

    $ git remote add origin https://vprogramador@bitbucket.org/parkout/api-parkout.git
    $ git push -u origin --all
    $ git push -u origin --tags

# Configuración del API

Creamos una nueva rama en la que vamos a realizar el trabajo relacionado
con el issue `BUS-26 Configurar API`

    $ git checkout -b BUS-26-Configurar-API

## Configurar Base de Datos PostgreSQL

se crean las bases de datos para desarrollo y pruebas, y el usuario.  

    $ createdb api-gap_development
    $ createdb api-gap_test

se actualiza el archivo `config/database.yml`  

    diff --git a/config/database.yml b/config/database.yml
    index cbbf1ed..bdc8838 100644
    --- a/config/database.yml
    +++ b/config/database.yml
    @@ -23,7 +23,9 @@ default: &default
     
     development:
       <<: *default
    -  database: apidb_development
    +  database: api_dev
    +  username: admin
    +  password: admin
     
       # The specified database role being used to connect to postgres.
       # To create additional roles in postgres see `$ createuser --help`.
    @@ -57,7 +59,9 @@ development:
     # Do not set this db to the same as development or production.
     test:
       <<: *default
    -  database: apidb_test
    +  database: api_test
    +  username: admin
    +  password: admin
     
     # As with config/secrets.yml, you never want to store sensitive information,
     # like your database password, in your source code. If your source code is
    @@ -80,6 +84,6 @@ test:
     #
     production:
       <<: *default
    -  database: api-vecino-libre_production
    -  username: api-vecino-libre
    +  database: apivecino-libre_production
    +  username: apivecino-libre
       password: <%= ENV['API-VECINO-LIBRE_DATABASE_PASSWORD'] %>

## Iniciar Gemfile

Se agregan al Gemfile las gemas que vamos a utilizar en nuestra API.  

Se debe tener en cuenta que algunas gemas solo las necesitamos en
determinados ambientes de trabajo.  

    source 'https://rubygems.org'

    ruby '2.2.0'

    gem 'rails', '4.2.4'
    gem 'rails-api', '0.4.0'
    gem 'rack-cors', '0.4.0'
    gem 'active_model_serializers', '0.8.3'
    gem 'activesupport-json_encoder', '1.1.0'
    gem 'fabrication', '2.1.0'
    gem 'ffaker', '2.1.0'
    gem 'pg', '0.18.3'

    group :development, :test do
      gem 'rspec-rails', '3.3.3'
    end

    group :production do
      gem 'rails_12factor', '0.0.3'
      gem 'puma', '2.14.0'
    end

Corremos `bundle update` para instalar las gemas.  


## Configurar [CORS](http://www.w3.org/TR/cors/)

Agregamos la configuración de la gema `rack-cors` en el archivo `config/application.rb`:  

    ## CORS
    config.middleware.use Rack::Cors do
      allow do
        origins '*'
        resource '*',
        :headers => :any,
        :expose  => ['access-token', 'expiry', 'token-type', 'uid', 'client'],
        :methods => [:get, :post, :options, :delete, :put, :patch]
      end
    end

## Configurar JSON encoder

Agregamos la configuración de la gema `activesupport-json_encoder` en
`config/application.rb`:   

    # Avoid integers to be wrapped in double quotes in JSON serialization
    ActiveSupport::JSON::Encoding.encode_big_decimal_as_string = false

## Configurar Fabrication

[Fabrication](http://www.fabricationgem.org/) es una gema que nos permite crear objetos. La vamos a utilizar para crear datos de prueba para los tests.

Configuramos la gema en `config/application.rb`

    ## Fabrication
    config.generators do |g|
      g.test_framework :rspec, fixture: true
      g.fixture_replacement :fabrication
    end

## Configurar RSpec

[Rspec](https://en.wikipedia.org/wiki/RSpec) es un framework de [desarrollo dirigido por comportamiento](https://en.wikipedia.org/wiki/Behavior-driven_development) escrito para Ruby.  

Primero generamos los archivos de configuración

    $ rails generate rspec:install

Este comando nos genera los archivos

  * `.rspec`
  * `spec/spec_helper.rb`
  * `spec/rails_helper.rb`

A continuación configuramos el formato de salida de los tests agregando la
línea `--format documentation` al archivo `.rspec`.  

Luego agregamos la línea `require 'rails_helper'` en `spec/spec_helper.rb`.  

## Configurar Mailer

Agregamos la configuración del servidor de correo para el mailer en `config/environments/development.rb` y `config/environments/production.rb` teniendo en cuenta los diferentes valores para el parámetro `host`.  

    diff --git a/config/environments/development.rb b/config/environments/development.rb
    index b55e214..02c0575 100644
    --- a/config/environments/development.rb
    +++ b/config/environments/development.rb
    @@ -13,8 +13,9 @@ Rails.application.configure do
       config.consider_all_requests_local       = true
       config.action_controller.perform_caching = false
     
    -  # Don't care if the mailer can't send.
    -  config.action_mailer.raise_delivery_errors = false
    +  # Mailer
    +  config.action_mailer.delivery_method = :test
    +  config.action_mailer.raise_delivery_errors = true
     
       # Print deprecation notices to the Rails logger.
       config.active_support.deprecation = :log

**Se debe pasar el parámetro password al archivo de configuración utilizando variables de entorno para no dejar la clave escrita en un archivo que se subirá al entorno de producción.**

    diff --git a/config/environments/production.rb b/config/environments/production.rb
    index 5c1b32e..53befe0 100644
    --- a/config/environments/production.rb
    +++ b/config/environments/production.rb
    @@ -1,6 +1,21 @@
     Rails.application.configure do
       # Settings specified here will take precedence over those in config/application.rb.
     
    +  # Mailer
    +  config.action_mailer.raise_delivery_errors = true
    +  config.action_mailer.delivery_method = :smtp
    +  host = 'https://api-vecino-libre.herokuapp.com'
    +  config.action_mailer.default_url_options = { host: host }
    +  config.action_mailer.smtp_settings = {
    +    address: "smtp.zoho.com",
    +    port: 465,
    +    domain: "vecino.com.co",
    +    authentication: :login,
    +    user_name: "user@mail.com",
    +    password: "password",
    +    ssl: true,
    +  }
    +
       # Code is not reloaded between requests.
       config.cache_classes = true

## Implementar ruta y controlador de prueba

Para probar el API  de forma local con un navegador, generamos un controlador
de prueba

    $ rails g controller Inicio --no-controller-specs

    class InicioController < ApplicationController
      def index
        @prueba = "API.\nLa fecha de hoy es: #{DateTime.now}"
        render json: @prueba
      end
    end

y configuramos la ruta raíz en `config/routes.rb` para que apunte a una acción en él.

    Rails.application.routes.draw do
      root 'inicio#index'
    end

Corremos el API de forma local con `rails s` y verificamos que en la
raíz obtenemos el objeto JSON generado por el controlador.

Se corre `rspec` y se hace commit.

## Configurar Producción

Para subir el API a un entorno de producción primero debemos habilitar la conexión segura con SSL agregando la línea `config.force_ssl = true` en `config/environments/production.rb`.

Creamos el archivo de configuración del servidor Puma `config/puma.rb`

    workers Integer(ENV['WEB_CONCURRENCY'] || 2)
    threads_count = Integer(ENV['MAX_THREADS'] || 5)
    threads threads_count, threads_count

    preload_app!

    rackup      DefaultRackup
    port        ENV['PORT']     || 3000
    environment ENV['RACK_ENV'] || 'development'

    on_worker_boot do
      # Worker specific setup for Rails 4.1+
      # See: https://devcenter.heroku.com/articles/deploying-rails-applications-with-the-puma-web-server#on-worker-boot
      ActiveRecord::Base.establish_connection
    end

Para correr el servidor Puma en producción tenemos que agregar un archivo `Procfile` con las instrucciones.

    web: bundle exec puma -C config/puma.rb

Hacemos commit, merge y subimos a Heroku

    $ git add -A
    $ git commit -m "Configura Producción"
    $ git checkout master
    $ git branch
    $ git merge 00-configurar-api
    $ git branch -d 00-configurar-api
    $ heroku login
    $ heroku create api-vecino-libre
    $ git push --set-upstream heroku master