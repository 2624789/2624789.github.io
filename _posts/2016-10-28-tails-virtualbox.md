---
layout: post
title: "Tails Virtualbox"
date: 2016-03-19 10:26:04 -0500
categories: crypto
---

Es posible utilizar Tails en una máquina virtual utilizando [VirtualBox](https://www.virtualbox.org/) desde un sistema operativo anfitrión Windows, Linux, o Mac OS X.

*Utilizar Tails en una máquina virtual tiene [varias implicaciones en seguridad](https://tails.boum.org/doc/advanced_topics/virtualization/index.en.html#security). Dependiendo del sistema operativo anfitrión y sus necesidades de seguridad, utilizar Tails en una máquina virtual podría ser peligroso.*

VirtualBox tiene una versión que es software libre, llamada VirtualBox Open Source Edition y algunos componentes privativos, por ejemplo para agregar soporte para dispositivos USB.

Por razones de seguridad, se recomienda que use únicamente la versión Open Source anque ésta no permita usar una unidad persistente.

Con la funcionalidad de VirtualBox llamada [carpetas compartidas](https://www.virtualbox.org/manual/ch04.html#sharedfolders) puede acceder archivos en su sistema anfitrión desde el sistema virtualizado.

Asegúrese de que entiende las implicaciones en seguridad de [acceder discos duros internos](https://tails.boum.org/doc/encryption_and_privacy/your_data_wont_be_saved_unless_explicitly_asked/index.en.html) desde Tails antes de utilizar esta característica.

Consideraciones de seguridad para Windows y Mac OS X
====================================================

En nuestras [advertencias de seguridad sobre virtualización](https://tails.boum.org/doc/advanced_topics/virtualization/index.en.html#security) recomendamos utilizar Tails en una máquina virtual sólo si el sistema operativo anfitrión es de confianza.

Microsoft Windos y Mac OS X, siendo software privativo, no pueden ser considerados de confianza. Sólo utilice Tails en una máquina virtual en Windows o OS X para pruebas y no dependa de ella para su seguridad.

Instalación
===========

Para instalar **VirtualBox** en Debian o Ubuntu, ejecute el siguiente comando:

    sudo apt-get install virtualbox

Para instrucciones sobre cómo instalar **VirtualBox** en otros sistemas operativos, vaya a la [documentación de VirtualBox](https://www.virtualbox.org/wiki/End-user_documentation).

Ejecutar Tails desde una imagen ISO
===================================

Primero, inicie **VirtualBox**.

Para crear una nueva máquina virtual:

  1. Elija **Máquina ▸ Nueva...**.
  1. En la ventana **Nombre y sistema operativo**, especifique:
     - Un nombre de su elección.
     - **Tipo**: **Linux**.
     - **Versión**: **Other Linux (32 bit)**.
     - Click en **Siguiente**.

     Los módulos de VirtualBox ofrecen características adicionales cuando se usa Tails en una máquina virtual: carpetas compartidas, monitor ajustable, portapapeles compartido, etc.
     Pero debido a un [bug en VirtualBox](https://www.virtualbox.org/ticket/11037), el monitor ajustable y el portapapeles compartido solo funcionan en Tails si la máquina virtual está configurada para tener un procesador de 32 bit. Las carpetas compartidas funcionan con máquinas de 32 y 64 bit.

  1. En la ventana **Tamaño de memoria**:
     - Asigne al menos 1024 MB de RAM.
     - Click en **Siguiente**.
  1. En la ventana **Disco duro**:
     - Escoja **No agregar un disco duro virtual**.
     - Click en **Crear**.
     - Click en **Continuar** en el mensaje de advertencia sobre crear una máquina virtual sin un disco duro.

Para configurar la máquina virtual para que inicie desde la imagen ISO:

  1. Seleccione la nueva máquina virtual en el panel izquierdo.
  1. Vaya a **Máquina ▸ Configuración...**
  1. Seleccione **Almacenamiento** en el panel izquierdo.
  1. Seleccione **Vacío** abajo de **Controlador: IDE** en la lista de seleción **Árbol de almacenamiento** del panel derecho.
  1. De click en el ícono **CD** a la derecha de la ventana y luego en **Seleccione archivo de disco óptico virtual...** para buscar la imagen ISO desde la cual se quiere iniciar Tails.
  1. Marque la opción **CD/DVD vivo**.
  1. Click en **OK**.

Para arrancar la nueva máquina virtual:

  1. Seleccione la máquina virtual en el panel izquierdo.
  1. Click **Iniciar**.

Traducción al español del artículo original [VirtualBox](https://tails.boum.org/doc/advanced_topics/virtualization/virtualbox/index.en.html).
