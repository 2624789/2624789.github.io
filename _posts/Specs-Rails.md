## API con autenticación

Si estamos probando un API con autenticación debemos ser capaces de
manejar las sesiones de los usuarios en nuestros tests para probar los
diferentes contextos en los que el usuario está logueado o no, o que el
usuario logueado es de un tipo específico.  

### Ejemplo

En este caso vamos a probar un API que solo permite el acceso a la gestión
de los recursos a usuarios logueados que sean administradores.  

La prueba para ver un recurso sería la siguiente:
