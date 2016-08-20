Son objetos sustituibles enlazados utilizando [inyección de
dependencias](). Se utilizan para compartir código a través de la
aplicación.  

Los servicios sólo se instancian cuando un componente de la aplicación
lo solicita como dependencia. Cada componente que depende del servicio
recibe una referencia a la única instancia generada por el servicio
factory.  

Para utilizar un servicio simplemente se agrega como una dependencia
para el componente (controlador, servicio, filtro o directiva).  

## Crear un Servicio

En angular se pueden definir servicios propios registrando el nombre del
servicio y la función factory del servicio en un módulo.  

La función factory del servicio genera el objeto único o la función que
representa al servicio en el resto de la aplicación. Este objeto o
función retornado por el servicio es inyectado en cualquier componente
que especifique como dependencia el servicio.  

    app.factory('carrito', function() {
      var productos = []

      var carrito = {
        agregarProducto: agregarProducto,
      }

      return carrito

      // Agrega un producto al carrito
      // Recibe como parámetros:
      //  productoId: id del producto
      //  precio: el precio o el arreglo del precios del producto
      //  vendedorId: el id de quien vende el producto, usuario o negocio
      // No devuelve nada
      function agregarProducto(productoId, precio, vendedorId) {
        console.log('agrega Producto')
      },
    })

## Más info

  * [Servicios Angular]()
  * [Guía de Estilo](http://www.johnpapa.net/angular-style-guide/)
