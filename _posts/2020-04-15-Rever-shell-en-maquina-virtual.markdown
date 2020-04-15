---
layout: post
title:  "Reverse shell en maquina virtual"
date:   2020-04-15 01:11:00 +0100
categories: Hacking
---

# Reverse shell en maquina virtual

## El problema
<br />Cuando se realizan ataques de explotación de vulnerabilidades que permiten establecer una sesión sobre la maquina victima, normalmente se utilizan payload los cuales nos brindan una shell de linea de comandos de la victima sobre nuestra maquina atacante.
<br />Algunos profesionales de la seguridad suelen realizar este tipo de ataques desde una maquina virtual con las herramientas necesarias para ello (Kali, Parrot, BlackArch, etc). Esta maquina virtual suelen ejecutarla desde un equipo con sistema operativo Microsoft Windows.
<br />Dependiendo de los permisos que se tengan en la organización a la cual se le esta realizando la auditoria, un atacante podria configurar la tarjeta de red virtual en modo puente/Bridge, lo cual permitiria tomar direccionamiento propio de la compañia y facilitar el tipo de ataques.
<br />Sin embargo, en la mayoria de las compañias, se tiene direccionamiento IP Statico o reserva DCHP desde la MAC del equipo, y adicionalmente solo proporcionan una dirección IP desde la cual se ejecutan las pruebas de seguridad. Personalmente recomiendo tener el equipo con dual boot, para poder realizar las pruebas de seguridad directamente desde un sistema operativo Linux, aunque esto en algunas ocasiones no puede realizarse por limitaciones propias del cliente, en las cuales únicamente permite realizar conexiones desde sistemas operativos Windows a tavés de NAC u otros controles de seguridad.
<br />Retomando los ataques de sesión remota, al ejecutar el exploit (manual o automático) normalmente se recibe la sesión en el sistema operativo desde el cual se realiza la explotación. Por ejemplo desde Metasploit Framework, al ejecutar este tipo de ataques se establece una sesión de meterpreter o una reverse_tcp configurando la IP de la maquina atacante en la opción LHOST y especificando el puerto en LPORT. Lo anterior configura automáticamente el puerto en escucha para establecer la sesión.
<br />Pero al realizar este tipo de ataques desde una maquina virtua con configuración de red NAT, la maquina virtual adquiere una dirección IP privada la cual no es visible desde la maquina victima, por lo que al configurar esta dirección como "Local host" en el exploit, no se establece la sesión debido a que no es visible para la victima.
## Posibles soluciones
<br />Al tener este problema, realice una busqueda en internet para poder dar una solución a la conexión en este tipo de escenarios. Al no encontrar una solución publicada en internet, pense en el tipo de solución que yo le daría a este problema. De esta manera se me ocurrieron dos posibles soluciones:
<br />- Configurar un port forwarding / re-dirección de puerto local desde la máquina host a la maquina virtual.
- Abrir un puerto de escucha desde el sistema operativo HOST Windows para recibir la sesión remota.
<br /><br />Para hacer un port forwarding que reciba la sesión y la envie a la máquina virtual requiere realizar la configuración en el sistema operativo host y poner el puerto en escucha en la máquina virtual para recibir la sesión desde el sistema operativo HOST. Sin embargo, se requieren menos configuraciones al simplemente poner a escuchar el puerto en la máquina host, por lo que opté por esta última dado que me pareció mas practica.
### Configuraciones previas a la explotación
<br />Para poder recibir la sesión remota, es importante que la victima tenga visibilidad del equipo desde el cual se realiza el ataque, por lo que se debe desactivar temporalmente el firewall de Windows y del Software antivirus (en caso de tener instalado) con el fin de garantizar la visibilidad.

<br />Adicionalmente, para recibir la sesión se debe hacer uso de un software que permita esta acción. En mi caso utilice Netcat para Windows el cual descargue del este [sitio](https://eternallybored.org/misc/netcat/), aunque existen varios otros sitios e incluso repositorios de GitHub en los cuales puede encontrarse.

<br />Para poner el puerto en escucha se mantiene la sintaxis de netcat:
	nc.exe -lvp [PUERTO]
### Ejecución del ataque 

### Conclusiones
