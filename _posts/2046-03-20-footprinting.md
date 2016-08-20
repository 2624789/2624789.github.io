---
layout: post
title: "Footprinting"
date: 2046-03-20 20:26:04 -0500
categories: seguridad
---

## ¿Qué es Footprinting?

**Footprinting** es el proceso de recolectar tanta información como sea posible acerca de un objetivo con el fin de identificar diferentes formas de acceder a sus sistemas de información.

  * **Footprinting Pasivo**: Consiste en recolectar información sobre un objetivo de fuentes de acceso público. No requiere contacto directo con el objetivo. Se puede obtener información sobre fronteras de red, IPs accesibles desde internet, sistemas operativos, servidores web, servicios TCP o UDP, mecanismos de control de acceso, arquitectura del sistema, sistemas de detección de intrusión y otras cosas más.
  * **Footprinting Activo**: Este proceso se enfoca principalmente en el componente humano del objetivo y trata de extraer información de las personas aplicando técnicas de ingeniería social por medio de visitas, entrevistas, cuestionarios y otras más.
  * **Footprinting Anónimo**: Consiste en recolectar información de fuentes anónimas de tal forma que no se pueda rastrear el proceso de recolección de información desde la fuente.
  * **Footprinting Seudónimo**: Consiste en recolectar información de fuentes que no están enlazadas directamente a su autor. Puede ser información que un autor publica bajo un nombre diferente.
  * **Footprinting Privado**: Recolección de información que incluye fuentes privadas.
  * **Footprinting de Internet**: Se refiere al proceso de recolección de información de las conexiones a internet del objetivo.

Realizar el proceso de Footprinting de una forma metodológica permite generar un bosquejo del perfil de seguridad del objetivo.

Se considera metodológico un proceso de footprinting en el que la información crítica se busca con base en un descubrimiento previo.

Un proceso de footprinting se puede realizar en cuatro pasos:

  1. Recolectar información básica sobre el objetivo y su red
  2. Determinar sistemas operativos que usa, plataformas, servidores web, etc.
  3. Aplicar técnicas como WHOIS, DNS, peticiones de red
  4. Encontrar vulnerabilidades y exploits para realizar ataques


### ¿Para qué el Footprinting?

El footprinting se utiliza para construir una estrategia de ataque, se recolecta información acerca del objetivo para encontrar la forma más fácil de acceder a sus sistemas de información. El footprinting ayuda a:

  * **Conocer el perfil de seguridad**: Obtener el perfil de seguridad del objetivo por medio de footprinting permite analizarlo en búsqueda de brechas de seguridad y de acuerdo a ello diseñar los planes de ataque.
  * **Reducir el área de ataque**: Utilizando diferentes técnicas y herramientas se puede reducir un objetivo a un rango específico de nombres de dominio, bloques de red o IPs individuales.
  * **Crear una base de datos de información**: un footprinting detallado ofrece suficiente información para construir una base de datos propia sobre las debilidades en seguridad del objetivo que puede ser analizada para la ejecución de ataques.
  * **Dibujar un mapa de red**: las técnicas de footprinting, en conjunto con herramientas como [Tracert](http://tracert.com/) permite crear diagramas de red del objetivo que pueden servir como guías para un ataque.

### Objetivos del Footprinting

Los principales objetivos del Footprinting son:

#### Información de Red

  * Nombres de dominio (internos y externos)
  * Bloques de red
  * Direcciones IP de los sistemas alcanzables
  * Sitios web privados/ocultos
  * Servicios activos TCP/UDP
  * Protocolos de red
  * Puntos VPN
  * Controles de acceso (ACLs)
  * Sistemas de detección de intrusión activos
  * Números telefónicos análogos/digitales
  * Mecanismos de autenticación

#### Información del Sistema

  * Nombres de grupos y usuarios
  * Banners de sistema
  * Tablas de enrutamiento
  * Información SNMP
  * Arquitectura de sistema
  * Tipos de sistemas remotos
  * Nombres de sisitemas
  * Contraseñas

#### Información de la Organización

  * Detalles de empleados
  * Sitio web de la organización
  * Directorio de la organización
  * Detalles de ubicación
  * Direcciones y números telefónicos
  * Comentarios en código HTML
  * Políticas de seguridad implementadas
  * Enlaces a servidores web relevantes
  * Información interna de la organización
  * Noticias, artículos, publicaciones de prensa

## Amenazas de Footprinting

Un atacante aplicando técnicas de footprinting puede obtener información valiosa sobre la red y el sistema. Esta información puede ser detalles de cuentas, sistemas operativos, aplicaciones instaladas, componentes de red, nombres de servidores o esquemas de bases de datos.

Las principales amenazas del footprinting son:

  * **Ingeniería Social**
  * **Ataques a la Red y el Sistema**
  * **Fugas de Información**
  * **Pérdida de Privacidad**
  * **Espionaje Corporativo**
  * **Pérdidas del Negocio**

## Metodología

### Footprinting con Motores de Búsqueda

Un motor de búsqueda está diseñado para buscar información en la Web, y permiten extraer información sobre un objetivo.

Una búsqueda en [Google](https://www.google.com) puede mostrar publicaciones en foros de personal de seguridad donde se revelan marcas de firewalls o sistemas de antivirus utilizados por el objetivo.

Para buscar información sobre un objetivo lo primero que se debe hacer es ingresar su nombre en un motor de búsqueda y examinar los resultados. Se puede hacer más precisa la búsqueda agregando otros términos específicos.

Estas búsquedas pueden mostrar información sobre la ubicación física, dirección de contacto, servicios ofrecidos, empleados, etc.

Incluso si se ha eliminado información sensible de un objetivo, puede que esta información aún permanezca almacenada en el caché del motor de búsqueda.

#### URLs Internas/Externas

Las URLs externas son las que se usan por fuera de la red de la organización para acceder a sus servidores a través de un firewall. Estas se pueden encontrar ingresando el nombre del objetivo en un motor de búsqueda.

Las URLs internas se utilizan para acceder a los servidores de la organización directamente dentro de su red. La mayoría de organizaciones utilizan formatos comunes para sus URLs internas, por lo que si se conoce la URL externa, se puede predecir una URL interna por ensayo y error. Estas URLs internas proveen datos de los diferentes departamentos o unidades de negocio de la organización.

Herramientas para buscar URLs internas:

  * [Netcraft](http://www.netcraft.com/)
  * [Link Extractor](http://www.iwebtool.com/link_extractor)

#### Sitios Públicos/Restringidos

  * **Sitio Público**: Está diseñado para mostrar la presencia de una
  organización en internet. Se utiliza para atraer clientes y socios.
  Contiene información como la historia de la organización, productos,
  servicios.
  * **Sitio Restringido**: Sólo está disponible para algunas personas de
  la organización. Las restricciones se pueden aplicar a direcciones IP,
  dominio o subred, usuario, contraseña.

#### Información de Ubicación

Utilice [Google Maps](https://www.google.com.co/maps) para obtener
información de la localización del objetivo. También es de bastante
utilidad obtener información acerca de puntos de acceso a WiFi públicos
cercanos.

Cuando se conoce la localización de un objetivo se pueden realizar
prácticas de recolección de información no técnicas como [dumpster
diving](https://es.wikipedia.org/wiki/Recolecci%C3%B3n_urbana), vigilancia o ingeniería social. También se pueden obtener
imágenes detalladas del lugar utilizando [Google Street
View](https://www.google.com/maps/streetview/). Esta
información sirve para obtener acceso no autorizado a edificios, redes
inalámbricas o cableadas, y otros sistemas, y también sirve para encontrar
cámaras de seguridad, puertas, puntos débiles en enrejados, conexiones
eléctricas o sitios para ocultarse.

#### Búsqueda de Personas

Se puede encontrar información acerca de un individuo en **sitios web de
búsqueda de personas**. A través de estos sitios se puede obtener
información como dirección de residencia, correo electrónico, números de
contacto, fecha de nacimiento, fotos y perfiles de redes sociales,
posts, e incluso imágenes satelitales de residencias privadas.

Esta información puede ser útil para detalles de cuentas bancarias,
tarjetas de crédito, números de celular.

Sitios para buscar personas:

  * [Pipl](https://pipl.com/)
  * [Spokeo](http://www.spokeo.com/)
  * [Zaba Search](http://www.zabasearch.com/)
  * [ZoomInfo](http://www.zoominfo.com/)
  * [AnyWho](http://www.anywho.com/whitepages)
  * [PeekYou](http://www.peekyou.com/)
  * [PeopleSmart](https://www.peoplesmart.com/)
  * [WhitePages](http://www.whitepages.com/)

También se puede encontrar información de individuos en servicios de
**redes sociales**, estos son servicios, plataformas o sitios web que se
enfocan en facilitar la construcción de redes sociales entre las
personas. La información que ofrecen estos sitios es ingresada por las
mismas personas. Las diferentes personas están directa o indirectamente
relacionadas unas con otras por intereses comunes, trabajo, ubicación o
comunidades educativas.

Estos sitios permiten acceder a información en tiempo real. Ofrecen
datos sobre eventos futuros o en progreso, anuncios recientes,
invitaciones y otras cosas. Son una gran herramienta para buscar
personas e información relacionada. A través de estos servicios se puede
acceder a información crítica que puede ser de utilidad para ataques,
por ejemplo, de ingeniería social.

Servicios de Redes Sociales:

  * [Facebook](https://www.facebook.com/)
  * [LinkedIn](https://www.linkedin.com/)
  * [Google+](https://plus.google.com/)
  * [Instagram](https://www.instagram.com/)

#### Servicios Financieros

Los servicios financieros pueden ofrecer información útil como el valor
de las acciones de una organización, su perfil, o detalles de sus
competidores.

Muchos servicios financieros se basan en acceso web, haciendo
transacciones y permitiéndole a sus usuarios acceder a sus cuentas. Un
atacante podría obtener la información de acceso utilizando un [keylogger](https://es.wikipedia.org/wiki/Keylogger).

Servicios Financieros:

  * [Google Finance](https://www.google.com/finance)
  * [Yahoo! Finance](http://finance.yahoo.com//)

#### Sitios de Empleos

Un atacante puede obtener información valiosa sobre sistemas operativos,
versiones de software o detalles de la infraestructura de una organización utilizando sitios de búsqueda de empleos.

Según los requerimientos para un puesto de trabajo, como por ejemplo,
*Administrador de Red*, un atacante podría saber detalles sobre las
tecnologías utilizadas por la organización.

Usualmente se exploran estos sitios buscando información acerca de
**requerimientos para empleos**, **perfiles de empleados**, **información de
hardware** e **información de software**.

Sitios de Empleos:

  * [Monster Jobs](http://www.monster.com/)
  * [Carrer Builder](http://www.careerbuilder.com/)
  * [Dice](http://www.dice.com/)
  * [Usa Jobs](http://www.usajobs.gov/)
  * [Computrabajo](http://www.computrabajo.com/)
  * [El Empleo](http://www.elempleo.com/)
  * [Trabajando](http://www.trabajando.com/)

#### Monitorear Objetivos Usando Alertas

Las Alertas son servicios de monitoreo de contenido que ofrecen
información actualizada vía correo electrónico.

Estos servicios notifican a sus usuarios cuando contenidos de noticias,
blogs o grupos de discusión en la web, coinciden con sus términos de
búsqueda.

Servicios de Alertas:

  * [Yahoo!
  Alerts](https://policies.yahoo.com/us/en/yahoo/privacy/products/alerts/)
  * [Giga Alert](http://www.gigaalert.com/)
  * [Google Alerts](https://www.google.com/alerts)

### Footprinting de Sitios Web

Un atacante puede construir un mapa detallado de la estructura y
arquitectura de un sitio web sin activar sistemas de detección de
intrusiones ni levantas sospechas de los administradores, utilizando
herramienas como un [navegador
web](https://es.wikipedia.org/wiki/Navegador_web) y [telnet]().

Utilizando una herramienta como [Netcraft](), se puede obtener
información de un sitio web como su dirección IP, nombre registrado,
nombre y dirección del dueño del dominio, nombre de dominio, [host]()
del sitio, detalles del sistema operativo, etc.

Ninguna herramienta puede dar todos los detalles, por lo que siempre se
debe explorar el sitio web buscando información sobre:

  * El software utilizado y su versión
  * Sistema Operativo
  * Subdirectorios y parámetros
  * Nombres de archivos, rutas, nombres de bases de datos, consultas
  * Plataforma de scripting (ver las extensiones de los archivos:
  **.php**, **.asp**, **.jsp**, etc)
  * Detalles de contacto
  * Software de Administración de Contenidos

Herramientas como [Paros Proxy](http://sectools.org/tool/paros/), [Burp Suite](https://portswigger.net/burp/) o [Firebug](http://getfirebug.com/) permiten
ver las cabeceras de las peticiones HTTP acceder a información como:

  * Connection Status y Content Type
  * Accept
  * Last-Modified
  * X-Powered-By
  * Web Server

Se debe examinar el **código fuente** en busca de comentarios que puedan
ofrecer información sobre el sistema que soporta el sitio web, e incluso
detalles de contacto del administrador o desarrollador del sitio. Los
enlaces y las etiquetas de imágenes pueden mostrar la estructura del
sistema de archivos. Revise cómo se comportan los scripts cuando reciben
información no válida. Las cookies pueden dar información acerca del
servidor.

#### Clonar un Sitio Web

Clonar un sitio web es crear una réplica exacta del sitio web original,
para esto se utilizan herramientas que descargan el sitio web a una
carpeta local.

Clonar un sitio web al sistema local permite diseccionar el sitio para
identificar vulnerabilidades, también facilita identificar la estructura
de directorios del sitio y más información sin necesidad de hacer
múltiples peticiones al servidor.

Herramientas para clonar sitios web:

  * [HTTrack](http://www.httrack.com/)
  * [SurfOffline](http://www.surfoffline.com/)
  * [Webripper](http://www.calluna-software.com/webripper)
  * [wget](https://www.gnu.org/software/wget/)

#### Extraer Información de Archive

[Archive](https://archive.org/) permite ver versiones archivadas de sitios web. Esto permite
obtener información de un sitio web desde su creación. Esta información
se puede acceder incluso luego de haber sido eliminada del sitio web.

#### Monitorear Actualizaciones Web

Herramientas como [WebSite-Watcher](http://aignes.com/) rastrean las
actualizaciones y cambios en sitios web y automáticamente guarda las dos
últimas versiones del sitio en el disco.

### Fooprinting con Correos Electrónicos

Se puede extraer información de las comunicaciones por correo utilizando
las cabeceras de los mensajes y herramientas para rastrear correos.

#### Rastrear Correos

Rastrear un correo permite obtener información sobre la ubicación de un
individuo o realizar ingeniería social.

Utilizando herramientas para rastreo de correos se puede obtener la
siguiente información acerca de un individuo:

  * Geolocalización
  * Tiempo utilizado para leer el correo
  * Tipo de servidor utilizado por el destinatario
  * Saber qué enlaces de un correo se han visitado o no
  * Sistema operativo utilizado por el destinatario
  * Conocer si el correo ha sido reenviado

Herramientas para rastrear correos:

  * [eMailTrackerPro](http://www.emailtrackerpro.com/)
  * [Read Notify](http://www.readnotify.com/)
  * [DidTheyReadIt](http://www.didtheyreadit.com/)
  * [G-Lock Analytics](https://glockanalytics.com/)

#### Obtener Información de la Cabecera del Mensaje

La cabecera de un mensaje es una pieza de información que tiene todo
mensaje de correo electrónico y contiene detalles como:

  * Servidor de correo de quien lo envía
  * Fecha y hora en que fue recibido por el servidor de correo de quien
  lo envía
  * Sistema de autenticación utilizado por quien lo envía
  * Fecha y hora de envío
  * Un número único utilizado para identificar el mensaje
  * Nombre completo de quien lo envía
  * Dirección IP de quien lo envía
  * La dirección desde la cual fue enviado

Para ver la cabecera de un mensaje de correo se debe guardar el mensaje
en el disco y abrirlo con un editor de texto.

Existen herramientas como
[TraceEmail](http://www.ip-address.org/tracker/trace-email.php) que analizan las cabeceras y
extraen información.

### Inteligencia Competitiva

### Footprinting con Google

### Footprinting con WHOIS

### Footprinting con DNS

### Footprinting de Red

### Footprinting con Ingeniería Social

### Footprinting con Redes Sociales

## Herramientas

## Medidas de Protección

## Pruebas de Penetración
