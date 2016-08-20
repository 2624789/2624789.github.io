Vamos a suponer que tenemos dos modelos, Banco y Cuenta.

La relación entre estos modelos:

  * Un banco puede tener muchas cuentas
  * Cada cuenta pertenece a un banco

Para poder representar esta relación en Rails tenemos que agregar una
[llave foránea]() a Banco en el modelo Cuenta, ya que cada cuenta
pertenece a un banco.

    $ rails generate model Banco nombre:string
    $ rails generate model Cuenta numero:string banco:references

## Migración

Si ya existe un modelo Cuenta, se puede agregar la referencia a Banco con una migración:

    $ rails generate migration AddBancoToCuenta banco:references

La migración que nos genera Rails se debe ver así:

    class AddBancoToCuenta < ActiveRecord::Migration
      def change
        add_reference :cuentas, :banco, index: true, foreign_key: true
      end
    end

Si la migración es correcta, la aplicamos:

    $ rake db:migrate
    $ rake db:migrate RAILS_ENV=test

## Modelos

Para terminar de establecer la relación debemos especificarla en
los modelos de Rails.

en `app/models/banco.rb`:

    has_many :cuentas

en `app/models/cuenta.rb`:

    belongs_to :banco
