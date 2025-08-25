# TU PRIMER SONDA POR SOFTWARE EN CONTENEDORES LXC - RIPE ATLAS

```
Diagrama de dos sondas Ripe Atlas, contenedores LXC configurados con OpenWrt 24.10, server Debian 11.
```
![Diagrama de ejemplo Contenedor LXC con OpenWRT y Sonda Ripe Atlas ](https://github.com/geryescalier/ripeatlas/blob/main/imagenes/dlxcripeatlas.svg)

# INDICE
- [Introducción](#introducción)
   - [Importante](#importante)
   - [Licencia](#licencia)
- [Requisitos](#requisitos)
   - [Ejecución de comandos](#ejecución-de-comandos)
   - [Conexión a Internet](#conexión-a-internet)
   - [Ordenador](#ordenador)
   - [Cuenta de usuario RIPE NCC](#cuenta-de-usuario-ripe-ncc)
   - [LXC Requisitos de Hardware Básicos](#lxc-requisitos-de-hardware-básicos)
- [Instalar LXC en Debian 11](#instalar-lxc-en-debian-11)
   - [Crear un bridge de red para LXC](#crear-un-bridge-de-red-para-lxc)
- [Crear el contenedor LXC con OpenWRT](#crear-el-contenedor-lxc-con-openwrt)
   - [Configurar OpenWRT para obtener IP por DHCP](#configurar-openwrt-para-obtener-ip-por-dhcp)
- [Instalar en OpenWrt Ripe Atlas](#instalar-en-openwrt-ripe-atlas)
   - [Configurar sonda Ripe Atlas](#configurar-sonda-ripe-atlas)
   - [Pasos previos para registrar sonda](#pasos-previos-para-registrar-sonda)
   - [Comandos Ripe Atlas](#comandos-ripe-atlas) 
# Introducción 
Sitio web de [RIPE Atlas](https://atlas.ripe.net/) 
```
Con su ayuda, RIPE NCC está construyendo la red de medición de Internet más grande jamás creada. RIPE Atlas emplea 
una red global  de sondas que miden la conectividad y la accesibilidad de Internet, lo que brinda una comprensión 
sin precedentes del estado de Internet en tiempo real.
```
Sitio web  [RIPE Atlas en LACNIC](https://www.lacnic.net/1000/1/lacnic/ripe-atlas-en-latinoamerica-y-caribe) 
```
RIPE Atlas es una de las plataformas de medición de parámetros de red en Internet de mayor despliegue a nivel mundial.
Pone a disposición de sus miembros recursos que permiten realizar mediciones de redes. RIPE NCC es el organismo encargado
de llevar adelante este enorme proyecto y cuenta con la colaboración de LACNIC para la región de Latinoamérica y Caribe.

La plataforma está en expansión y enfoca sus esfuerzos en las zonas donde la penetración puede ser mejor: Asia, 
Latinoamérica, Caribe y África.
```
Esta documentación está diseñada para realizar pruebas paso a paso en laboratorios utilizando contenedores [LXC](https://linuxcontainers.org). El objetivo es configurar una sonda de software y registrarla en la plataforma. Se detallará la configuración de un router con [OpenWrt](https://openwrt.org/es/start), la instalación de paquetes de RIPE Atlas, el registro de la sonda en la plataforma de RIPE Atlas y las pruebas de funcionamiento correspondientes.

Utilizar contenedores LXC ofrece numerosas ventajas en comparación con el hardware físico. Permite crear tantas sondas como lo permita tu hardware, y puedes crear y destruir estas sondas tantas veces como sea necesario. Además, facilita la realización de demostraciones gráficas mediante scripts, lo que es ideal tanto para presentaciones como para resolver dudas. Esta flexibilidad es perfecta para llevar a cabo las pruebas que necesites.

¡Todos los aportes para mejorar esta documentación son muy bienvenidos! No dudes en realizar tu contribución. ¡Muchas gracias!

## Importante
> En Ripe Atlas recomiendan tener la sonda conectada todo el tiempo posible, esto permitira que la sonda reporte información constante y pueda ser consultada a futuro (información historica). Es muy importante que al terminar las pruebas que necesites realizar en tu laboratorio, tengas preparado uno o varios medios fisicos (Router, RaberryPi, Pc, otros) o logicos (gns3, docker, lxc, otros) para que alojes tu sonda y esta pueda estar operativa 24 x 7. 

## Licencia
Este trabajo está licenciado bajo [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

[![Creative Commons License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)

# Requisitos
## Ejecución de comandos
Todos los comandos que se verán a continuación se ejecutan en modo usuario con privilegios. Para evitar problemas al copiar y pegar los comandos, no se utilizará el símbolo # al inicio de cada línea. En su lugar, reitero que los comandos deben ejecutarse con privilegios de superusuario. 

Para ejecutar el comando, asegúrate de precederlo con sudo o de cambiar al usuario root antes de ejecutarlo. Por ejemplo:
```
sudo lxc-ls -f
```
```
su -
lxc-ls -f
```
## Conexión a Internet
No es necesaria una conexion a internet de alto ancho de banda, importante debe ser una conexion cableada, adsl o fibra.

## Ordenador 
Se recomienda un ordenador de mesa o portatil que tenga instalado distro Debian (GNU/Linux) o distro basada en Debian.

## Cuenta de usuario RIPE NCC
Crea una cuenta en https://access.ripe.net/registration  (tu usuario sera tu correo electrónico)

## LXC Requisitos de Hardware Básicos
- Procesador (CPU):Procesador 64 bits es recomendado, de 2 GHz o más es ideal. 
- Memoria (RAM):2 GB de RAM o más es ideal
- Almacenamiento: Un disco duro o SSD. Al menos 10 GB de espacio libre en el disco. Esto incluye espacio para el sistema operativo host, LXC, y los contenedores que planeas ejecutar.
- Tipo: Un procesador de 64 bits es recomendado, aunque algunos sistemas de 32 bits pueden funcionar con LXC.
- Velocidad: Un procesador de al menos 1 GHz es suficiente para contenedores básicos. Para un rendimiento mejorado, un procesador de 2 GHz o más es ideal.
- Memoria (RAM): Recomendado: 2 GB de RAM o más es ideal para un funcionamiento suave, especialmente si planeas ejecutar varios contenedores simultáneamente.
- Almacenamiento: Un disco duro o SSD.
  - Espacio: Al menos 10 GB de espacio libre en el disco. Esto incluye espacio para el sistema operativo host, LXC, y los contenedores que planeas ejecutar. Para contenedores más grandes o múltiples contenedores, considera tener más espacio disponible.
- Conexión: Una conexión a Internet estable es recomendada para descargar imágenes de contenedores y actualizaciones. Red: Interfaz de Red: Asegúrate de que tu sistema tenga una interfaz de red funcional para que los contenedores puedan comunicarse con la red externa.

# Instalar LXC en Debian 11
Actualiza el sistema:
```
apt update
apt full-upgrade
```
Instala LXC:
```
apt install lxc
```
## Crear un bridge de red para LXC
Instala el paquete bridge-utils:
```
apt install bridge-utils
```
Crea un bridge de red llamado br0:
```
brctl addbr br0
```
Agrega la interfaz física eth0 al bridge (reemplaza eth0 con la interfaz que estés utilizando):
```
brctl addif br0 eth0
```
Configura la IP del bridge (si lo deseas):
```
ip addr add 192.168.1.100/24 dev br0
```
Activa el bridge:
```
ip link set br0 up
```
Edita el archivo /etc/network/interfaces para que el bridge se active automáticamente al arrancar el sistema:
```
auto br0
iface br0 inet dhcp
    bridge_ports eth0
    bridge_stp off
    bridge_fd 0.0
```
Reinicia el servicio de red:
```
systemctl restart networking
```
# Crear el contenedor LXC con OpenWRT
Descarga la imagen de OpenWRT 24.10 para LXC:
```
lxc-create -n openwrt -t download -- --dist openwrt --release 24.10 --arch amd64
```
Configura el contenedor para utilizar el bridge br0:
```
sed -i 's/lxc.network.type = empty/lxc.network.type = veth\nlxc.network.link = br0\nlxc.network.flags = up/' /var/lib/lxc/openwrt/config
```
Inicia el contenedor:
```
lxc-start -n openwrt
```
Verifica que el contenedor esté funcionando:
```
lxc-ls -f
```
## Configurar OpenWRT para obtener IP por DHCP
Accede al contenedor:
```
lxc-attach -n openwrt
```
Edita el archivo de configuración de la red /etc/config/network para que tenga el siguiente contenido:
```
config interface 'lan'
    option ifname 'eth0'
    option proto 'dhcp'
```
Reinicia el servicio de red:
```
service network restart
```
Verifica que OpenWRT haya obtenido una IP por DHCP:
```
ip addr show eth0
```
# Instalar en OpenWrt Ripe Atlas
- Necesitamos instalar los paquetes de Ripe Atlas para que trabajen en tu Router, continuamos desde el paso anterior, ingresa comando
```
opkg update
```
El anterior comando actualiza los paquetes de tu Router, ahora podemos realizar la instalacion de los paquetes de Ripe Atlas, ingresa el comando
```
opkg install atlas-sw-probe
```

## Configurar sonda Ripe Atlas
- Ahora podemos configurar los paquetes de Ripe Atlas, ingresa comando 
```
vi /etc/atlas/atlas.readme
```
Nos mostrara la siguiente información (en ingles) se ha realizado algunos cambios con respecto al readme original.
```
Para salir del archivo  /atlas.readme selecciona las teclas ":q" sin las comillas y la tecla enter.
```
El software de la sonda atlas requiere una clave rsa 2048-4096 para el registro.

## Pasos previos para registrar sonda

- Inserta tu nombre de usuario en el archivo de configuración de atlas, usa el comando
```
vi /etc/config/atlas
```
- Selecciona tecla "i" para insertar 
- Debes ubicar el cursor (usando las flechas de tu teclado) en la linea, option username 'ingresa aqui tu nombre usuario' (tu usuario es el correo electronico con el que creaste tu cuenta en la pagina de RIPE NCC)
- Presiona tecla "Esc"
- Para guardar y salir presiona las teclas ":wq"  >>> sin las comillas.
- Crearemos una clave priv/pub. Usa el comando
```
/etc/init.d/atlas create_key
```
La clave priv/pub se almacenará en el directorio /etc/atlas/

Usa el comando
```
/etc/init.d/atlas get_key 
```
para obtener la clave pública utilizada para el registro de la sonda.
```
Asegúrate de copiar la clave completa y que el último valor sea el nombre de usuario correcto.
```
- Cuando termines de realizar las configuraciones anteriores, ejecuta los siguientes comandos:
``` 
/etc/init.d/atlas start
```
```
/etc/init.d/atlas enable
```
```
/etc/init.d/atlas enabled
```
```
/etc/init.d/atlas status 
```
solo en ultimo comando (status) veras un mensaje "running" esto confirma que se esta ejecutando Ripe Atlas en tu router.

## Comandos Ripe Atlas
Mira los comandos disponibles ingresando el comando
```
/etc/init.d/atlas 
```
<pre>
Comandos disponibles:

- start      Iniciar el servicio
- stop       Detener el servicio
- restart    Reiniciar el servicio
- reload     Recargar archivos de configuración (o reiniciar si el servicio no implementa la recarga)
- enable     Habilitar el inicio automático del servicio
- disable    Deshabilitar el inicio automático del servicio
- enabled    Comprobar si el servicio se inicia en el arranque
- running    Comprobar si el servicio se está ejecutando
- status     Estado del servicio
- trace      Comenzar con rastreo de llamada al sistema
- get_key    imprime la clave pública de la sonda (utilizada para el registro de la sonda)
- probeid    imprime el id de la sonda
- log        imprimir registro de estado de la sonda
- load_backup 'backup.tar.gz' carga la clave ssh de copia de seguridad desde tar.gz
- create_key  crea la clave priv/pub de la sonda
</pre>
Ejemplo :
```
# /etc/init.d/atlas start
running
#
```
# Registro de sonda por software
Debes tener activa tu sesión en la pagina de RIPE NCC, ingresa en https://atlas.ripe.net/apply/swprobe/

Ingresaras al formulario para completar los datos.

## AS Number
Para conocer tu [Sistema Autonomo](https://es.wikipedia.org/wiki/Sistema_aut%C3%B3nomo) (AS number) debes identificar cual es la ip publica que tienes asignada.
### Conocer tu IP pública y ASN desde la web
- Ingresa en https://www.ripe.net/ en la parte inferior del todo de la pagina, veras:
```
Your IP address is: xx.xx.xx.xx <<< mostrara tu ip pública
```
- Da click a tu ip (Your IP address is: xx.xx.xx.xx ) te llevara a una nueva pagina.
- En la nueva pagina busca  la sección, origin:  
```
origin:         AS (AQUI MOSTRARA LA NUMERACION DE TU AS)
```
- En el formulario solo debes poner los números de tu AS
```
origin:          AS12757
```
- En el formulario solo debes ingresar 12757

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

Tu país 

## Public Key
- En tu router openwrt, usa el comando 
```
/etc/init.d/atlas get_key 
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
