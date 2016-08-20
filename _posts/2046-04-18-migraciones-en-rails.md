Las migraciones en Rails nos permite hacer modificaciones al esquema de base de datos.

## Quitar Columna

		$ rails g migration RemoveTypeFromSpots type:integer

Genera la siguiente migración:

		class RemoveTypeFromSpots < ActiveRecord::Migration
		  def change
		    remove_column :spots, :type, :integer
		  end
		end

## Agregar Columna

		$ rails g migration AddOwnerTypeFromSpots owner_type:integer

Genera la siguiente migración:

		class AddOwnerTypeFromSpots < ActiveRecord::Migration
		  def change
		    add_column :spots, :owner_type, :integer
		  end
		end


