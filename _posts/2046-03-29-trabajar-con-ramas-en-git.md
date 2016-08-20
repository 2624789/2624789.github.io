## Scrum

Vamos a utilizar las [ramas]() de git para organizar el trabajo del
equipo siguiendo un esquema de historias de usuario.

Cuando se va a empezar a trabajar en una historia de usuario, lo primero
que se hace es crear una nueva rama para la historia que empieza.

    $ git checkout -b MCH-02-Historia-de-usuario
    $ git push --set-upstream origin MCH-02-Historia-de-usuario

## Commits

Los commits permiten llevar un registro de los cambios que se le hacen
al código.

    $ git commit -m "MCH-02 #time 1h - Crea #script para automatizar pruebas"

### `--amend`

Si se quiere corregir el comentario del último commit realizado:

    $ git commit --amend -m "MCH-02 #time 2h - Crea #script para automatizar pruebas"

## Pull Request

Cuando un miembro del equipo considera que ha finalizado la
implementación de una funcionalidad, hace un Pull Request o *solicitud
de integración*, para que la funcionalidad que escribió en su rama sea
agregada a la rama principal.

### Integración

Hacemos una copia local de la rama que contiene la nueva funcionalidad

    $ git fetch && git checkout nueva-funcionalidad

Se deben correr los specs y sólo si todos pasan, se continúa con la
revisión del código.

    $ git diff --stat master..nueva-funcionalidad
    $ git diff master..nueva-funcionalidad spec/requests/transacciones_spec.rb

Si en la revisión se considera que el código está listo, se integra a la
rama principal.

    $ git checkout master
    $ git merge nueva-funcionalidad

Si el código no está listo, se rechaza la solicitud de integración
especificando lo que se debe hacer con el código para que se pueda
integrar a la rama principal.

### Conflictos

Es posible que se presenten conflictos cuando se intenta realizar una
integración. Principalmente porque existen archivos que se han
modificado en las dos ramas.

    $ git merge otra-rama
    Auto-merging spec/requests/transacciones_spec.rb
    CONFLICT (content): Merge conflict in spec/requests/transacciones_spec.rb
    Automatic merge failed; fix conflicts and then commit the result.

podemos ver más detalles del proceso de integración con `git status`.

Para poder realizar la integración se deben revisar los archivos
conflictivos y editarlos para darle solución a los conflictos.

    $ vim spec/requests/transacciones_spec.rb

Git utiliza tres marcadores para indicar los conflictos entre las ramas
a integrar:

  * **`<<<<<<<`**: Indica el comienzo de las líneas que tuvieron
    conflicto. El primer conjunto de líneas son las líneas que hay en el
    archivo al que se quiere integrar los cambios.
  * **`=======`**: Indica el punto de separación para poder realizar la
    comparación. Arriba quedan lo que había en el archivo antes de
    intentar realizar la integración, y abajo quedan las líneas que se
    quieren agregar al archivo pero que tuvieron conflicto.
  * **`>>>>>>>`**: Indica el final de las líneas que tuvieron conflicto.

Luego de resolver los conflictos se corren los specs y todos deben estar
en verde.

    $ rspec spec/requests/transacciones_spec.rb
    $ rspec

Se agrega el archivo modificado a los cambios y se realiza el commit

    $ git commit -add spec/requests/transacciones_spec.rb
    $ git commit -m ""

Luego se verifica que todo sigue funcionando correctamente con `rspec`.
