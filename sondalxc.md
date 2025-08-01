# TU PRIMER SONDA POR SOFTWARE EN CONTENEDORES LXC - RIPE ATLAS

![Diagrama de ejemplo Contenedor LXC con OpenWRT y Sonda Ripe Atlas ](https://github.com/geryescalier/ripeatlas/blob/main/imagenes/diagramalxcripeatlas.svg)

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
Esta documentación esta pensada para poder realizar pruebas paso a paso bajo laboratorios en [LXC](https://linuxcontainers.org), para configurar una sonda por software y su registro. Se detallara la configuración de un router con [OpenWrt](https://openwrt.org/es/start), la instalación de paquetes de Ripe Atlas, registro de la sonda en la plataforma de Riple Atlas y sus correspondientes pruebas de funcionamiento.
