## Zoho

Para tener cuentas de correo con un dominio propio, vamos a utilizar el servicio de correo electrónico que ofrece [Zoho](https://www.zoho.com/) porque nos permite hacerlo de forma gratuita.

Vamos a la [página de registro](https://mail.zoho.com/biz/mailsignup.do?plan=free) e ingresamos el dominio para el que queremos crear las cuentas de correo. Podemos poner uno que ya hayamos adquirido, o comprar uno si aún no tenemos.

Luego creamos la cuenta con la que vamos a administrar el servicio

Debemos tener una cuenta de correo activa para realizar el proceso de registro.

### Verificar Dominio

Para empezar a configurar el dominio, primero tenemos que verificarle a Zoho que el dominio es nuestro. Una forma de hacerlo es creando un [registro CNAME]() en el dominio con un código de verificación de Zoho:

Para crear el registro CNAME, tenemos que ir a la administración de nuestro dominio, para esto tenemos que ir a la configuración DNS del dominio y crear un registro cuyo nombre sea el código de verificación que nos da zoho.

### Configurar Dominio

Primero elegimos el nombre de usuario para la cuenta administrador, que puede ser el mismo con el que nos registramos en zoho

Luego creamos las otras cuentas de usuario, debemos habilitarle el acceso [IMAP]()

### Configurar DNS

Para poder utilizar el dominio en el servicio de correo, se deben crear entradas [MX]() en el DNS

### Configurar Cliente

https://www.zoho.com/mail/help/thunderbird-imap-access.html