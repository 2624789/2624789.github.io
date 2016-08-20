Módulo para interactuar con APIs REST que implementa patrones Active
Record para mapear datos desde APIs REST a objetos. Permite crear
colecciones de modelos y construir relaciones de forma automática.  

## Relaciones

Restmod permite establecer conexiones entre objetos de "uno a uno" o de
"uno a muchos".  


## Configuración

Para configurar Restmod se utiliza el sistema `mixins`, para agregarle a
la definición de los modelos configuraciones generales (`$config`),
ganchos a eventos (`$hooks`), extensiones del objeto como métodos (`$extend`),
además de configuraciones específicas para cada campo.  

Cada una de estas configuraciones se puede hacer de manera global o por
cada modelo.  

La configuración global permite establecer el nombre de la llave
primaria de los modelos, la URL del API y el envoltorio de los datos.  

## Ganchos `$hooks`

Son como los interceptores de Angular, permiten modificar las peticiones
antes de enviarlas, cambiar la respuesta cuando llega, controlar el
procesamiento de la respuesta o manejar de forma global errores del API.  

Existen opciones para antes y despúes de muchos eventos, como crear,
error al crear, guardar, eliminar, enviar petición. [Más
info](http://platanus.github.io/angular-restmod/tutorial-hooks.html).  

## Extensiones `$extend`

Permite sobreescribir la lógica implementada por Restmod agregando
nuevos métodos a modelos, colecciones o a la interfaz estática.  

## Ejemplos

### Auth Token

    module.config(['restmodProvider',
      function (restmodProvider) {
        restmodProvider.rebase({
          $hooks: {
            'before-request': function(request) {
              request.headers["Auth-Token"] = token
              return request
            }
          }
        })
      }
    ])

### Definición de un modelo

    module.factory('ModeloProducto', function(restmod) {
      restmod.model('/productos').mix({
        $config: { name: 'producto' },
      })
    })

### Uso del modelo

    var jugo = ModeloProducto.$find(1)
    // GET: /productos/1

    jugo.$then(function(model) {
      console.log(model.presentacion)
      // > "600 ml"
    })

### Crear objeto en el API

    var nuevoProducto = ModeloProducto.$create({
      nombre: "Pan"
    })
    // POST: /productos

### Editar objeto en el API

    nuevoProducto.imagen = "/img/pan.png"

    nuevoProducto.$save()
    // PUT
    nuevoProducto.$save(['imagen'])
    // PATCH

### Traer colecciones de modelos

    var productos = ModeloProducto.$collection()
    productos.$fetch()
    // GET: /productos

### Traer partes de colecciones de modelos

    var productos = ModeloProducto.$search({
      categoria: "Desayuno"
    }).$refresh()
    // GET: /productos?categoria=Desayuno

### Agregar métodos a modelos

    module.factory('ModeloProducto', function(restmod) {
      return restmod.model('/productos').mix({
        $extend: {
          Record: {
            menorPrecio: function() {
              var preciosOrdenados = this.precios.sort(function(a,b) {
                return a.precio - b.precio
              })
              return preciosOrdenados[0].precio
            },
          }
        }
      })
    })

### Configuración de campos

    module.factory('ModeloProducto', function(restmod) {
      return restmod.model('/productos').mix({
        imagen: { init: "img/producto.png" },
        usuario: { hasOne: "ModeloUsuario" },
        precios: { hasMany: "ModeloPrecio" }
      })
    })

    var precios = jugo.precios.$fetch()
    // GET: /productos/1/precios

    var usuario = jugo.usuario.fetch()
    // GET: /productos/1/usuario
