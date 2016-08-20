Rails utiliza las formas en plural o singular del nombre del modelo para
determinar los nombres de los archivos que genera.

Como Rails está hecho en inglés, tiene capacidades limitadas para
manejar nombres en español, y a veces toca decirle cuál es la forma
singular o plural de una palabra, principalmente cuando utilizamos
modelos con nombres compuestos.

Si queremos crear un modelo de nombre **TitularCuenta**, Rails nos
creará archivos como `spec/models/titular_cuentum_spec.rb`, cuando el
nombre debería ser algo como **TitularesCuentas**, los plurales de las
dos palabras.

    [WARNING] The model name 'TitularCuenta' was recognized as a plural,
    using the singular 'TitularCuentum' instead. Override with
    --force-plural or setup custom inflection rules for this noun before
    running the generator.

Para enseñarle a Rails cuál es el singular o plural de alguna palabra,
tenemos que crear reglas de inflexión en `config/environment.rb`:

    ActiveSupport::Inflector.inflections do |inflect|
      inflect.irregular 'titular_cuenta', 'titulares_cuentas'
      inflect.irregular 'TitularCuenta', 'TitularesCuentas'
    end

En general, Rails debería ser capaz de hallar el plural o singular de
la mayoría de las palabras del español con este conjunto de reglas:

    ActiveSupport::Inflector.inflections do |inflect|
      inflect.clear

      inflect.plural(/$/, 's')
      inflect.plural(/([^aeéiou])$/i, '\1es')
      inflect.plural(/([aeiou]s)$/i, '\1')
      inflect.plural(/z$/i, 'ces')
      inflect.plural(/á([sn])$/i, 'a\1es')
      inflect.plural(/é([sn])$/i, 'e\1es')
      inflect.plural(/í([sn])$/i, 'i\1es')
      inflect.plural(/ó([sn])$/i, 'o\1es')
      inflect.plural(/ú([sn])$/i, 'u\1es')

      inflect.singular(/s$/, '')
      inflect.singular(/es$/, '')
      inflect.singular(/([sfj]e)s$/, '\1')
      inflect.singular(/ces$/, 'z')

      inflect.irregular('el', 'los')
    end
