# Whitelist

Implementa una política de autorización para la navegación de la
aplicación. Nos permite decirle a la aplicación que conexiones puede
establecer o a qué páginas puede navegar. [Más
info](https://github.com/apache/cordova-plugin-whitelist)  

    $ ionic plugin add cordova-plugin-whitelist

En `config.xml` agregamos las etiquetas para la configuración del
plugin  

    <access origin="https://api.ejemplo.com" />
