## Modelo

Para generar un modelo en [Ruby on Rails](), utilizamos la sentencia
`generate model`, y a continuación indicamos el nombre del modelo que se
quiere obtener. Este nombre debería ir en singular e iniciar con
mayúscula, ejemplo: `generate model Vehiculo`.

Podemos ser más específicos y caracterizar los atributos del modelo
uno a uno.

Es bueno escribir la sentencia en un archivo temporal y una vez se tenga
la versión definitiva, ejecutar este archivo en consola.

    $ vim comando

    ## comando
    rails g model Vehiculo \
      placa:string{6}:uniq \
      marca:string{50} \
      modelo:string{50} \
      tipo:integer \

Se ejecuta el archivo:

    $ cat comando | sh

Editamos la migración generada para agregar detalles de campos
requeridos, o valores por defecto.

Para crear las tabla en la base de datos aplicamos la migración

    $ rake db:migrate
    $ rake db:migrate RAILS_ENV=test

Si faltó algo por agregar, y se necesita corregir la migración, se hace `rollback`, se modifica la migración y luego se aplica de nuevo.

    $ rake db:rollback
    $ rake db:rollback RAILS_ENV=test


## Pruebas Unitarias

Escribimos un listado de las pruebas unitarias del modelo, es decir, las
condiciones que deben cumplir los atributos de los objetos creados a
partir del modelo.

    context "Atributos válidos" do
      it "es válido con todos sus atributos"
    end

    context "Atributos no válidos" do
      it "No es válido sin placa"

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
