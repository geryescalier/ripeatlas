# TU PRIMER SONDA POR SOFTWARE - RIPE ATLAS 

# Introducción 
Sitio web de [RIPE Atlas](https://atlas.ripe.net/) 
```
Con su ayuda, RIPE NCC está construyendo la red de medición de Internet más grande jamás creada. 
RIPE Atlas emplea una red global  de sondas que miden la conectividad y la accesibilidad de Internet, 
lo que brinda una comprensión sin precedentes del estado de Internet en tiempo real.
```
Sitio web  [RIPE Atlas en LACNIC](https://atlas.ripe.net/) 
```
RIPE Atlas es una de las plataformas de medición de parámetros de red en Internet de mayor despliegue
a nivel mundial.Pone a disposición de sus miembros recursos que permiten realizar mediciones de redes.
RIPE NCC es el organismo encargado de llevar adelante este enorme proyecto y cuenta con la colaboración
de LACNIC para la región de Latinoamérica y Caribe.

La plataforma está en expansión y enfoca sus esfuerzos en las zonas donde la penetración puede ser mejor: 
Asia, Latinoamérica, Caribe y África.
```

Esta documentación esta pensada para poder realizar pruebas bajo laboratorios en [GNS3](https://www.gns3.com/software), relacionadas a las sondas por software, tener un punto de partida, paso a paso, 
tanto de prueba como en producción. Entre otras pruebas que puedes realizar, crear tu laboratorio previo a configurar tu sonda en tu propio router con openwrt y
tener tu sonda trabajando en producción.

Usar GNS3 te permite tener muchas opciones rapidas por software, a diferencia de hacerlo por hardware, crear tantas sondas como lo permita 
tu ordenador (hardware), crear y destruir tus sondas tanta veces como lo necesites, realizar explicaciones mostrando el laboratorio de manera grafica, tanto para desmostraciones como para resolver dudas, es ideal para realizar las pruebas que necesites!

Todos los aportes para mejorar esta documentación son muy bienvenidos! no dudes en realizar tu aporte por favor, muchas gracias.

## Licencia
Este trabajo está licenciado bajo [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

[![Creative Commons License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)


# Requisitos
```
Todos los comandos que se verán en adelante, se ejecutan en modo usuario con privilegios (# aqui el comando)  
```

## Conexión a Internet
No es necesaria una conexion a internet de alto ancho de banda, importante debe ser una conexion cableada, adsl o fibra.

## Ordenador con OS  Debian o basado en Debian
Ordenador de mesa o portatil que tenga instalado distro Debian o distro basada en Debian.

## Cuenta de usuario RIPE NCC
Crea una cuenta en https://access.ripe.net/registration  (tu usuario sera tu correo electrónico)

## Tener instalado GNS3
Hardware básico recomendado: Procesador 4 núcleos, RAM 4GB, espacio libre en Disco 5GB

## Imagen Quemu de Openwrt 
Configuraremos con esta imagen un router con openwrt, esto nos permitira instalar los paquetes de Ripe Atlas.

Descarga en https://downloads.openwrt.org/releases/21.02.0-rc4/targets/x86/64/

Descargar imagen >>> generic-ext4-combined.img.gz

Descomprimir 
gunzip generic-ext4-combined.img.gz

Redimensionar
qemu-img resize generic-ext4-combined.img.gz 300M

Teniendo estos requisitos estamos preparados para inciar las configuraciones en GNS3

///////////////////////////////////////////

Configurar en GNS3
==================
Iniciar GNS3
Menú > Edit > Preferences
QUEMU > Quemu VMS
Selecciona New 
Name Ripe Atlas > Siguiente 

Quemu binary <---> RAM (dejamos ambas opciones seleccionadas por defecto) > Siguiente 

Telnet > Siguiente 
Seleccionamos New Image > selecionamos Browse > buscamos nuestra imagen redimensionada > Mensaje > seleccionamos Si 
Seleccionamos Finalizar
Seleccionamos Aceptar

Crear laboratorio 
=================
Seleccionamos en menú principal, File > New blank project
Desde el menu izquierdo (iconos de dispositivos) seleccionamos > Browse all device:
Arrastrar Nube a laboratorio para tener acceso a internet
Arrastrar Switch genérico a laboratorio
Arrastrar imagen (router con openwrt) de Ripe Atlas a laboratorio 

Configurar router para acceso a internet
=================
Nube conectar con switch genérico >  switch genérico con router (Openwrt) Ripe Atlas > Iniciar Router Ripe Atlas > durante el inicio dar dos veces a tecla enter.

Configurar red en router
ingresar en  # vi /etc/config/network

Letra ·i· para insertar contenido

(abajo veras un ejemplo, deberás configurar según los datos de tu red local)

config interface 'lan' 

        option device 'br-lan'
        option proto 'static'
        option ipaddr '192.168.0.20'
        option netmask '255.255.255.0'
        option ip6assign '60'
        option gateway '192.168.0.1'
        option dns  '1.1.1.1'

terminada actualización con tus datos, presionamos tecla Esc, luego  teclas ":wq" sin comillas, esto dirá al editor vi que guarde cambios y salir.

Reiniciamos servicio con comando # service network reload

Prueba a dns externo 
Ingresa comando # ping 1.1.1.1 

Ejemplo de respuesta a pin:
# ping 1.1.1.1
PING 1.1.1.1 (1.1.1.1): 56 data bytes
64 bytes from 1.1.1.1: seq=0 ttl=64 time=1.414 ms
64 bytes from 1.1.1.1: seq=1 ttl=64 time=1.426 ms

// si tenemos repuesta hemos finalizado.


Instalar en Openwrt Ripe Atlas
======================

Ingresa comando # opkg update
Ingresa comando # opkg install atlas-sw-probe


Configurar sonda Atlas
========================
Ingresa comando # vi /etc/atlas/atlas.readme

Instrucciones de configuración de la sonda Atlas

Mira los comandos disponibles ingresando: 
# /etc/init.d/atlas 

Comandos disponibles:
         start      Iniciar el servicio
         stop       Detener el servicio
         restart    Reiniciar el servicio
         reload     Recargar archivos de configuración (o reiniciar si el servicio no implementa la recarga)
         enable     Habilitar el inicio automático del servicio
         disable    Deshabilitar el inicio automático del servicio
         enabled    Comprobar si el servicio se inicia en el arranque
         running    Comprobar si el servicio se está ejecutando
         status     Estado del servicio
         trace      Comenzar con rastreo de llamada al sistema
         get_key    imprime la clave pública de la sonda (utilizada para el registro de la sonda)
         probeid    imprime el id de la sonda
         log        imprimir registro de estado de la sonda
         create_backup copia de seguridad de la clave ssh para tar.gz
         load_backup 'backup.tar.gz' carga la clave ssh de copia de seguridad desde tar.gz
         create_key  crea la clave priv/pub de la sonda

El software de la sonda atlas requiere una clave rsa 2048-4096 para el registro.

Sigue estos pasos previos para registrar tu sonda en los sistemas ripe-atlas:
 Inserta tu nombre de usuario en el archivo de configuración de atlas, usa el comando # vi /etc/config/atlas
  Selecciona tecla "i" para insertar 
  En la linea, option username 'ingresa aqui tu nombre usuario'
  Presiona tecla "Esc"
  Para guardar y salir presiona las teclas ":wq"  >>> sin las comillas.
  
 Usa el comando # /etc/init.d/atlas create_key  para crear una clave priv/pub. La clave priv/pub se almacenará en el directorio /etc/atlas/
 Usa el comando # /etc/init.d/atlas get_key para obtener la clave pública utilizada para el registro de la sonda.
 Asegúrate de copiar la clave completa y que el último valor sea el nombre de usuario correcto.

Cuando termines las configuraciones anteriores, debes ejecutar los siguientes comandos:
 
# /etc/init.d/atlas start
# /etc/init.d/atlas enable
# /etc/init.d/atlas enabled
# /etc/init.d/atlas status <<< solo en este comando veras un mensaje "running" esto confirma que tienes corriendo Ripe Atlas en openwrt.
 
Registrar sonda
===================
Debes tener activa tu sesión en la pagina de RIPE, ingresa en https://atlas.ripe.net/apply/swprobe/

Ingresaras al formulario para completar los datos.

AS Number:
-----------
Para conocer tu AS number debes identificar cual es la ip que tienes asignado, puedes usar https://www.lacnic.net/1002/1/lacnic/whois (en esta web veras tu ip asignada en la parte superior derecha "Su dirección IP es:" ) u otro sitio web de tu preferencia.

Desde tu cli favorito usa el comando seguido de tu ip:

# whois (aqui tu ip)

Busca la linea que ponga:

origin:         AS(AQUI MOSTRARA LA NUMERACION DE TU AS)

En el formulario solo debes poner los números de tu AS

City:
------
Tu ciudad

Country:
-------
Tu país 

Public Key:
------------
Usa el comando 
# /etc/init.d/atlas get_key 

Notes:
---------
Alguna referencia general con la que quieras identificar tu sonda: ·Sonda Mi universidad // Sonda Mi Zona // Otros·

I accept the RIPE Atlas Service Terms and Conditions:
--------------
Lee y acepta los términos y condiciones (el recuadro debe estar seleccionado)


Para enviar el formulario selecciona > Submit your application

Recibirás los siguientes correos:

Thank you for applying for a RIPE Atlas software probe! (¡Gracias por solicitar una sonda de software RIPE Atlas!)

Recibirás un segundo correo en unos 15 a 30 minutos (el tiempo puede variar): Your new RIPE Atlas software probe is created (Se ha creado tu nueva sonda de software RIPE Atlas)

Ingresa en https://atlas.ripe.net/ con tu usuario activo, selecciona > Probes and Anchors > Probes > podrás ver información de tu sonda activa.

















