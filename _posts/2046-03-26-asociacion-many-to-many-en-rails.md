Vamos a suponer que tenemos dos modelos, Cuenta y Transacción.

La relación entre estos modelos:

  * Una Cuenta puede tener muchas transacciones
  * Una Transacción puede pertenecer a muchas cuentas

Para poder representar esta relación en Rails tenemos que usar una [tabla conjunta](),
y agregar una [llave foránea]() a cada Modelo.

## Migraciones

Generamos una migración para crear la tabla conjunta

    $ rails generate migration CreateJoinTableExtractos cuentas transacciones

La migración que nos genera Rails se debe ver así:

    class CreateJoinTableExtractos < ActiveRecord::Migration
      def change
        create_join_table :cuentas, :transacciones do |t|
          t.index [:cuenta_id, :transaccion_id]
          t.index [:transaccion_id, :cuenta_id]
        end
      end
    end

Si la migración es correcta, la aplicamos:

    $ rake db:migrate
    $ rake db:migrate RAILS_ENV=test

Si no es correcta, Rollback:

    $ rake db:rollback RAILS_ENV=test
    $ rake db:rollback
    $ rails d migration CreateJoinTableExtractos cuentas transacciones

## Modelos

También podemos crear la asociación partiendo desde un modelo:

    $ rails generate model Extracto \
        cuenta:references \
        transaccion:references

Revisamos la migración generada y si todo está bien la aplicamos.

Para terminar de definir la relación entre debemos especificarla en
los modelos asociados.

en `app/models/extracto.rb`

    belongs_to :cuenta
    belongs_to :transaccion

en `app/models/transaccion.rb`:

    has_many :extractos
    has_many :cuentas, trough: :extractos

en `app/models/cuenta.rb`:

    has_many :extractos
    has_many :transacciones, trough: :extractos

## Pruebas

Las validaciones de la asociación las hacemos en el modelo de la misma, es por esto que no utilizamos `has_and_belongs_to_many`

    $ rails generate rspec:model Extracto