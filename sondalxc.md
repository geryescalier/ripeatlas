# TU PRIMER SONDA POR SOFTWARE EN CONTENEDORES LXC - RIPE ATLAS

![Diagrama de ejemplo Contenedor LXC con OpenWRT y Sonda Ripe Atlas ](https://github.com/geryescalier/ripeatlas/blob/main/imagenes/diagramalxcripeatlas.svg)

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
