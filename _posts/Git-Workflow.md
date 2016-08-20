# Feature Branch Workflow

El flujo de trabajo [Feature
Branch](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow) establece que el desarrollo de cada historia de usuario o funcionalidad se debe realizar en su propia rama, no en la `master`.  

## Crear Rama

### Desde Jira

Podemos crear una nueva rama desde las opciones en Jira. Esta rama se
creará en el origen, por lo que debemos traerla al repositorio local
para empezar a trabajar en su implementación  

    $ git fetch && git checkout BUS-31-editar-perfil

### Desde el repositorio local

También podemos crear la rama en nuestro repositorio local y luego
subirla al `origin`  

    $ git checkout -b BUS-31-editar-perfil
    $ git push

## Pull Request con Bitbucket

Una vez se ha escrito el código de la historia de usuario o
funcionalidad, y se ha verificado que el código pasa todas las pruebas,
se sube la rama con los nuevos commits al `origin`  

    $ git push

Luego se realiza un Pull Request, una solicitud para integrar el nuevo código
en la rama `master` utilizando la interfaz de Bitbucket.  

### Merge Pull Request en Bitbucket

Al crear el Pull Request, Bitbucket notifica al administrador del
repositorio, y este puede revisar la solicitud desde la interfaz web,
por medio de la cual puede interactuar con el equipo de desarrollo antes
de integrar los cambios a la rama `master`.  

#### Pull Request no se puede integrar

Si por algún motivo el Pull Request no se puede integrar con la
`master`, se hacen comentarios sobre el código y se interactúa con el
equipo de desarrollo, para que se modifique el código hasta que se pueda
integrar al `master`.  

Una vez se ha decidido aceptar la solicitud, el administrador del
repositorio puede integrar el nuevo código a la rama `master` utilizando
el botón **Merge**.  

Luego de integrar el nuevo código a la `master`, el equipo de desarrollo
debe actualizar sus copias locales haciendo un pull  

    $ git pull

### Merge Pull Request en Local

Si se quiere hacer una revisión más detallada del Pull Request, el
administrador del repositorio puede hacer una copia local de la rama que
se quiere integrar para correr diferentes pruebas  

    $ git fetch && git checkout BUS-32-crear-modelos-en-api

Se verifica que el código se pueda integrar con la rama `master`  

    $ rspec

Y se integra con la rama `master`  

    $ git checkout master
    $ git merge BUS-32-crear-modelos-en-api

Se verifica que la integración se realizó sin problemas  

    $ rspec

Y se sube la `master` con el nuevo código al `origin`  

    $ git push

Ahora el equipo de desarrollo actualiza sus repositorios locales  

    $ git pull


