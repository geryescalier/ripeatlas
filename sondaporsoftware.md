# TU PRIMER SONDA POR SOFTWARE - RIPE ATLAS 
INDICE
- [Introducción](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md#Introducción)
   - [Importante](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##Importante)
   - [Licencia](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##Licencia)
- [Requisitos](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md#Requisitos)
   - [Conexión a Internet](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##Conexión_a_Internet)
   - [Ordenador](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##Ordenador)
   - [Cuenta de usuario RIPE NCC](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##Cuenta_de_usuario_RIPE_NCC)
   - [GNS3](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##GNS3)
   - [Imagen Quemu de Openwrt](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##Imagen_Quemu_de_Openwrt)   
- [Configuración de GNS3](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md#Configuración_de_GNS3)
- [Crear laboratorio](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md#Crear_laboratorio)
- [Configurar router para acceso a internet](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md#Configurar_router_para_acceso_a_internet)
   - [Configurar red en router](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##Configurar_red_en_router)   
- [Instalar en Openwrt Ripe Atlas](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md#Instalar_en_OpenWrt_Ripe_Atlas)
- [Configurar sonda Ripe Atlas](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md#Configurar_sonda_Ripe_Atlas)
    - [Pasos previos para registrar sonda](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##Pasos_previos_para_registrar_sonda)   
    - [Comandos Ripe Atlas](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##Comandos_Ripe_Atlas)   
- [Registro de sonda por software](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md#Registro_de_sonda_por_software)
    - [AS Number](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##AS_Number)   
      - [Conocer tu IP pública y ASN desde la web](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##Conocer_tu_IP_pública_y_ASN_desde_la_web)   
      - [Conocer tu ASN desde cli](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##Conocer_tu_ASN_desde_cli)  
    - [City](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##City)   
    - [Country](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##Country)   
    - [Public Key](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##Public_Key)   
    - [I accept the RIPE Atlas Service Terms and Conditions](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##I_accept_the_RIPE_Atlas_Service_Terms_and_Conditions)   
- [Fin](https://github.com/geryescalier/ripeatlas/edit/main/sondaporsoftware.md##Fin)   


# Introducción 
Sitio web de [RIPE Atlas](https://atlas.ripe.net/) 
```
Con su ayuda, RIPE NCC está construyendo la red de medición de Internet más grande jamás creada. RIPE Atlas emplea 
una red global  de sondas que miden la conectividad y la accesibilidad de Internet, lo que brinda una comprensión 
sin precedentes del estado de Internet en tiempo real.
```
Sitio web  [RIPE Atlas en LACNIC](https://atlas.ripe.net/) 
```
RIPE Atlas es una de las plataformas de medición de parámetros de red en Internet de mayor despliegue a nivel mundial.
Pone a disposición de sus miembros recursos que permiten realizar mediciones de redes. RIPE NCC es el organismo encargado
de llevar adelante este enorme proyecto y cuenta con la colaboración de LACNIC para la región de Latinoamérica y Caribe.

La plataforma está en expansión y enfoca sus esfuerzos en las zonas donde la penetración puede ser mejor: Asia, 
Latinoamérica, Caribe y África.
```

Esta documentación esta pensada para poder realizar pruebas paso a paso bajo laboratorios en [GNS3](https://www.gns3.com/software), para configurar una sonda por software y su registro. Se detallara la configuración de un router con [OpenWrt](https://openwrt.org/es/start), la instalación de paquetes de Ripe Atlas, registro de la sonda en la plataforma de Riple Atlas y sus correspondientes pruebas de funcionamiento.

Otra prueba que puedes realizar posterior a esta, es crear tu laboratorio para configurar tu sonda, basado en un modelo especifico de router que tengas y [sea compatible con openwrt](https://openwrt.org/es/toh/start) este tema no se vera en esta documentación.

Usar GNS3 te permite tener muchas opciones rapidas por software, a diferencia de hacerlo por hardware, crear tantas sondas como lo permita 
tu ordenador (hardware), crear y destruir tus sondas tanta veces como lo necesites, realizar explicaciones mostrando el laboratorio de manera grafica, tanto para desmostraciones como para resolver dudas, es ideal para realizar las pruebas que necesites!

Todos los aportes para mejorar esta documentación son muy bienvenidos! no dudes en realizar tu aporte por favor, muchas gracias.

## Importante
> En Ripe Atlas recomiendan tener la sonda conectada todo el tiempo posible, esto permitira que la sonda reporte información constante y pueda ser consultada a futuro (información historica). Es muy importante que al terminar las pruebas que necesites realizar en tu laboratorio, tengas preparado uno o varios medios fisicos (Router, RaberryPi, Pc, otros) o logicos (gns3, docker, lxc, otros) para que alojes tu sonda y esta pueda estar operativa 24 x 7. 

## Licencia
Este trabajo está licenciado bajo [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

[![Creative Commons License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)


# Requisitos

> Todos los comandos que se verán en adelante, se ejecutan en modo usuario con privilegios (# aqui el comando)  

## Conexión a Internet
No es necesaria una conexion a internet de alto ancho de banda, importante debe ser una conexion cableada, adsl o fibra.

## Ordenador 
Se recomienda un ordenador de mesa o portatil que tenga instalado distro Debian (GNU/Linux) o distro basada en Debian. GNS3 es multiplataforma puedes instalarlo en los sistemas operativos mas usados acutalmente.

## Cuenta de usuario RIPE NCC
Crea una cuenta en https://access.ripe.net/registration  (tu usuario sera tu correo electrónico)

## GNS3
GNS3 es multiplataforma puedes instalarlo en los sistemas operativos mas usados acutalmente. Se recomienda Hardware básico: Procesador 4 núcleos, RAM 4GB, espacio libre en Disco 5GB

## Imagen Quemu de Openwrt 
Configuraremos con esta imagen un router con openwrt, esto nos permitira instalar los paquetes de Ripe Atlas.

Descarga imagen Quemu desde https://downloads.openwrt.org/releases/21.02.0-rc4/targets/x86/64/

Descargar imagen >>> [generic-ext4-combined.img.gz](https://downloads.openwrt.org/releases/21.02.0-rc4/targets/x86/64/openwrt-21.02.0-rc4-x86-64-generic-ext4-combined.img.gz)  

En tu terminal, debes estar ubicado en el directorio donde tengas la imagen descargada, ejecuta comando.
```
:~$ gunzip generic-ext4-combined.img.gz
```
A continuación procedemos a redimensionar imagen de Quemu, ejecuta comando.
```
:~$ qemu-img resize generic-ext4-combined.img.gz 300M
```
Teniendo estos requisitos estamos preparados para inciar las configuraciones en GNS3

# Configuración de GNS3

- Inicia GNS3
- Selecciona Menú > Edit > Preferences
- Seleciona QUEMU > Quemu VMS
- Selecciona New 
- Name: Ripe Atlas > Siguiente 
- Quemu binary <---> RAM (deja ambas opciones seleccionadas por defecto) > Siguiente 
- Telnet > Siguiente 
- Selecciona New Image > seleciona Browse > busca imagen de quemu redimensionada > Mensaje > selecciona Si 
- Selecciona Finalizar
- Selecciona Aceptar

# Crear laboratorio 
- Selecciona en menú principal, File > New blank project
- Desde el menu izquierdo (iconos de dispositivos) selecciona > Browse all device:
- Arrastra Nube a laboratorio para tener acceso a internet
- Arrastra Switch genérico a laboratorio
- Arrastra imagen router Ripe Atlas a laboratorio 

# Configurar router para acceso a internet
- Selecciona en menú izquierdo, icono de cable utp (Add a link) 
- Conecta puerto libre de la nube con puerto libre de switch genérico
- Conecta puerto libre de switch genérico con puerto libre de router Ripe Atlas
- Cursor/puntero de mouse sobre router Ripe Atlas, presiona boton secundario de tu mouse para desplegar menú
- Selecciona Start, abrirara una nueva venta con conexion telnet, espera que inicie el router unos segundos, veras varias lineas con informacion del inicio de router, preciona la tecla enter en 2 ocasiones con un espacio de 3 segundos.
- Finalizados los pasos anteriores, te encuentras con acceso a tu router y esta preparado para ser configurado, abajo puedes ver un ejemplo:
```
BusyBox v1.33.1 (2021-10-24 09:01:35 UTC) built-in shell (ash)

  _______                     ________        __
 |       |.-----.-----.-----.|  |  |  |.----.|  |_
 |   -   ||  _  |  -__|     ||  |  |  ||   _||   _|
 |_______||   __|_____|__|__||________||__|  |____|
          |__| W I R E L E S S   F R E E D O M
 -----------------------------------------------------
 OpenWrt 21.02.1, r16325-88151b8303
 -----------------------------------------------------
=== WARNING! =====================================
There is no root password defined on this device!
Use the "passwd" command to set up a new password
in order to prevent unauthorized SSH logins.
--------------------------------------------------
root@OpenWrt:/#
 ```

## Configurar red en router
- Ya tienes acceso a tu router, ahora procede con las siguentes pasos para configurar el acceso a tu red y tengas conexion a internet, ingresa el comando
```
# vi /etc/config/network
```
Con el anterior comando te encuentras editando el archivo network con el editor Vim, selecciona la Letra ·i· para insertar contenido, ahora puedes ingresar contenido, usa las flechas para insertar tus datos (abajo veras un ejemplo, deberás configurar (sustituir) según los datos de tu red local)
```
config interface 'lan' 

        option device 'br-lan'
        option proto 'static'
        option ipaddr '192.168.0.20'
        option netmask '255.255.255.0'
        option ip6assign '60'
        option gateway '192.168.0.1'
        option dns  '1.1.1.1'
```
terminada la actualización del fichero con tus datos, presiona tecla Esc, luego  teclas ":wq" sin comillas, esto dirá al editor vim que guarde cambios y salga.

- Reiniciamos servicio, ingresa el comando
```
# service network reload
```
- Prueba a dns externo, ingresa comando
```
# ping 1.1.1.1 
```
- Ejemplo de respuesta a ping
```
# ping 1.1.1.1
PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=64 time=0.615 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=64 time=0.486 ms
64 bytes from 1.1.1.1: icmp_seq=3 ttl=64 time=0.504 ms
64 bytes from 1.1.1.1: icmp_seq=4 ttl=64 time=0.498 ms
64 bytes from 1.1.1.1: icmp_seq=5 ttl=64 time=0.495 ms
64 bytes from 1.1.1.1: icmp_seq=6 ttl=64 time=0.495 ms
64 bytes from 1.1.1.1: icmp_seq=7 ttl=64 time=0.527 ms
^C
--- 1.1.1.1 ping statistics ---
7 packets transmitted, 7 received, 0% packet loss, time 6152ms
rtt min/avg/max/mdev = 0.486/0.517/0.615/0.041 ms

```
Espera unos 5 segundos, para finalizar la prueba de ping seleciona las teclas Ctrl + c
-Si tienes un resultado similar al anterior, ya tienes configurado tu router con conexion a internet. 


# Instalar en OpenWrt Ripe Atlas
- Necesitamos instalar los paquetes de Ripe Atlas para que trabajen en tu Router, continuamos desde el paso anterior, ingresa comando
```
# opkg update
```
El anterior comando actualiza los paquetes de tu Router, ahora podemos realizar la instalacion de los paquetes de Ripe Atlas, ingresa el comando
```
# opkg install atlas-sw-probe
```

# Configurar sonda Ripe Atlas
- Ahora podemos configurar los paquetes de Ripe Atlas, ingresa comando 
```
# vi /etc/atlas/atlas.readme
```
Nos mostrara la siguiente información (en ingles) se ha realizado algunos cambios con respecto al readme original.
```
Para salir del archivo  /atlas.readme selecciona las teclas ":q" sin las comillas y la tecla enter.
```
El software de la sonda atlas requiere una clave rsa 2048-4096 para el registro.

## Pasos previos para registrar sonda

- Inserta tu nombre de usuario en el archivo de configuración de atlas, usa el comando
```
# vi /etc/config/atlas
```
- Selecciona tecla "i" para insertar 
- Debes ubicar el cursor (usando las flechas de tu teclado) en la linea, option username 'ingresa aqui tu nombre usuario' (tu usuario es el correo electronico con el que creaste tu cuenta en la pagina de RIPE NCC)
- Presiona tecla "Esc"
- Para guardar y salir presiona las teclas ":wq"  >>> sin las comillas.
- Crearemos una clave priv/pub. Usa el comando
```
# /etc/init.d/atlas create_key
```
 La clave priv/pub se almacenará en el directorio /etc/atlas/

- Usa el comando
```
# /etc/init.d/atlas get_key 
```
para obtener la clave pública utilizada para el registro de la sonda.
```
Asegúrate de copiar la clave completa y que el último valor sea el nombre de usuario correcto.
```
- Cuando termines de realizar las configuraciones anteriores, ejecuta los siguientes comandos:
``` 
# /etc/init.d/atlas start

# /etc/init.d/atlas enable

# /etc/init.d/atlas enabled

# /etc/init.d/atlas status 
```
solo en ultimo comando (status) veras un mensaje "running" esto confirma que se esta ejecutando Ripe Atlas en tu router.

## Comandos Ripe Atlas
Mira los comandos disponibles ingresando el comando
```
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
```
# Registro de sonda por software
Debes tener activa tu sesión en la pagina de RIPE NCC, ingresa en https://atlas.ripe.net/apply/swprobe/

Ingresaras al formulario para completar los datos.

## AS Number
Para conocer tu [Sistema Autonomo](https://es.wikipedia.org/wiki/Sistema_aut%C3%B3nomo) (AS number) debes identificar cual es la ip publica que tienes asignada.
### Conocer tu IP pública y ASN desde la web
- Ingresa en https://www.ripe.net/ en la parte superiro derecha veras 
```
Your IP address is: xx.xx.xx.xx <<< mostrara tu ip pública
```
- En la casilla donde indica "Search IP Address or ASN" ingresa tu ip pública, presiona la tecla enter o selecciona la lupa para que realice la busqueda de tu ASN
- La busqueda te lleva a una nueva pagina, busca la linea que pone:
```
origin:         AS (AQUI MOSTRARA LA NUMERACION DE TU AS)
```
- En el formulario solo debes poner los números de tu AS
```
origin:          AS12757

En el formulario solo debes ingresar 12757
```
### Conocer tu ASN desde cli

- Desde tu cli favorito usa el comando seguido de tu ip:
```
# whois (aqui tu ip)
```
- Busca la linea que ponga:
```
origin:         AS(AQUI MOSTRARA LA NUMERACION DE TU AS)
```
- En el formulario solo debes poner los números de tu AS
```
origin:          AS12757

En el formulario solo debes ingresar 12757
```
## City

Tu ciudad

## Country
```
Tu país 
```
## Public Key
- Usa el comando 
```
# /etc/init.d/atlas get_key 
```
obtendras la clave pública utilizada para el registro de la sonda.
- Asegúrate de copiar la clave completa y que el último valor sea el nombre de usuario correcto.

## Notes
Ingresa alguna referencia general con la que quieras identificar tu sonda, por ejemplo: Sonda Mi universidad // Sonda Mi Zona // Otros.

## I accept the RIPE Atlas Service Terms and Conditions
- Lee y acepta los términos y condiciones (el recuadro debe estar seleccionado)
- Para enviar el formulario selecciona > Submit your application

Recibirás los siguientes correos:
- Thank you for applying for a RIPE Atlas software probe! (¡Gracias por solicitar una sonda de software RIPE Atlas!)
- Recibirás un segundo correo en unos 15 a 30 minutos (el tiempo puede variar): Your new RIPE Atlas software probe is created (Se ha creado tu nueva sonda de software RIPE Atlas)
- Ingresa en https://atlas.ripe.net/ con tu usuario activo, selecciona > Probes and Anchors > Probes > podrás ver información de tu sonda activa.

# Fin
¡Felicidades ya tienes tu sonda configurada y registrada!

















