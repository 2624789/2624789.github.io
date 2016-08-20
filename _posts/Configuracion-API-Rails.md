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
    $ git config user.name "Parkout Programador"
    $ git config user.email "programador@parkout.com.co"
    $ git add -A
    $ git commit -m "Comienzo"

Después creamos un repositorio remoto en Bitbucket y lo asociamos al repositorio local

    $ git remote add origin https://vprogramador@bitbucket.org/parkout/api-parkout.git
    $ git push -u origin --all
    $ git push -u origin --tags

# Configuración del API

Creamos una nueva rama en la que vamos a realizar el trabajo relacionado
con el issue `BUS-26 Configurar API`

    $ git checkout -b Configurar-API

## Configurar Base de Datos PostgreSQL

se crean las bases de datos para desarrollo y pruebas, y el usuario.  

se actualiza el archivo `config/databases.yml`  

## Iniciar Gemfile

Lo primero que vamos a hacer es agregar al Gemfile las gemas que vamos a
utilizar en nuestra API.  

Se debe tener en cuenta que algunas gemas solo las necesitamos en
determinados ambientes de trabajo.  

    source 'https://rubygems.org'

    ruby '2.2.0'

    gem 'rails', '4.2.4'
    gem 'rails-api', '0.4.0'
    gem 'rack-cors', '0.4.0'
    gem 'activesupport-json_encoder', '1.1.0'
    gem 'pg', '0.18.3'

    group :development, :test do
      gem 'spring', '1.4.0'
      gem 'sqlite3', '1.3.10'
      gem 'rails-erd', '1.4.1'
      gem 'factory_girl_rails', '4.5.0'
      gem 'rspec-rails', '3.3.3'
    end

    group :production do
      gem 'rails_12factor', '0.0.3'
      gem 'puma', '2.14.0'
    end

Corremos `bundle install --without production` para instalar las gemas
sin tener en cuenta las del entorno de producción.  


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

## Configura Mailer

Agregamos la configuración del servidor de correo para el mailer en `config/environments/development.rb` y `config/environments/production.rb` teniendo en cuenta los diferentes valores para el parámetro `host`.  

**Se debe pasar el parámetro password al archivo de configuración utilizando variables de entorno para no dejar la clave escrita en un archivo que se subirá al entorno de producción.**

## Implementar ruta y controlador de prueba

Para probar el API  de forma local con un navegador, generamos un controlador
de prueba

    $ rails g controller pruebas --no-controller-specs

    class PruebasController < ApplicationController
      def index
        @prueba = "Parkout API.\nLa fecha de hoy es: #{DateTime.now}"
        render json: @prueba
      end
    end

y configuramos la ruta raíz en `config/routes.rb` para que apunte a una acción en él.

    Rails.application.routes.draw do
      root 'pruebas#index'
    end

Corremos el API de forma local con `rails s` y verificamos que en la
raíz obtenemos el objeto JSON generado por el controlador.

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

