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
<br />
* Configurar un port forwarding / re-dirección de puerto local desde la máquina host a la maquina virtual.
* Abrir un puerto de escucha desde el sistema operativo HOST Windows para recibir la sesión remota.
<br /><br />Para hacer un port forwarding que reciba la sesión y la envie a la máquina virtual requiere realizar la configuración en el sistema operativo host y poner el puerto en escucha en la máquina virtual para recibir la sesión desde el sistema operativo HOST. Sin embargo, se requieren menos configuraciones al simplemente poner a escuchar el puerto en la máquina host, por lo que opté por esta última dado que me pareció mas practica.
### Configuraciones previas a la explotación
<br />Para poder recibir la sesión remota, es importante que la victima tenga visibilidad del equipo desde el cual se realiza el ataque, por lo que se debe desactivar temporalmente el firewall de Windows y del Software antivirus (en caso de tener instalado) con el fin de garantizar la visibilidad.
<br />Adicionalmente, para recibir la sesión se debe hacer uso de un software que permita esta acción. En mi caso utilice Netcat para Windows el cual descargue del este [sitio](https://eternallybored.org/misc/netcat/){:target="_blank"}, aunque existen varios otros sitios e incluso repositorios de GitHub en los cuales puede encontrarse.
<br />Para este ejemplo, explotaremos la famosa vulnerabilidad de Eternalblue a través de Metasploit, también conocida por el boletín de seguridad correspondiente de Microsoft MS17-010 la cual fue utilizada en el ataque de ransomware WannaCry.
Para este escenario se tienen las siguientes maquinas:
1. Windows 10: Sistema operativo Host  
		Dirección IP:
2. Windows 7: Maquina vitual en Virtual Box - Victima  
		Configuracion de red NAT/direccionamiento de la red local.  
		Dirección IP:
3. Kali Linux: Maquina virtual en VMware - Atacante  
		Configuración de red en Bridge/Visible desde el sistema operativo host.  
		Dirección IP:

<br />Se virtualizan las dos maquinas en diferentes software de virtualización con el fin de garantizar que no haya visibilidad al atacante desde la máquina victima.
### Ejecución del ataque
<br />
___NOTA: En este post únicamente se explica como establecer la sesión remota en el sistema operativo host, por lo que no se detallan las configuraciones del exploit Eternalblue, este ataque se encuentra documentado en varios sitios de internet.___
<br />
1. Configurar puerto en escucha en Windows 10: *nc.exe -lvp [PUERTO]*
2. Configurar IP victima en opción RHOST
3. En la opción LHOST se configura la dirección IP del sistema operativo anfitrion (Windows 10) y en LPORT el puerto anteriormente configurado.
4. Ejecutar el exploit (run o exploit)  
<br />  
Como se puede observar se recibe correctamente la sesión en el sistema operativo HOST.  
### Conclusiones
<br />
Se ha podido establecer correctamente la sesión de una ataque de tipo shell reversa realizado desde una máquina virtual la cual no es visible desde la victima. Este metodo únicamente requiere la ejecución de Netcat para Windows y la desactivación de firewall en el sistema operativo, de esta manera se puede recibir la sesión remota en cualquier ataque de este tipo.
<br />
Si bien al principio del post indique que no encontré ninguna publicación en internet que explicará este proceso, también es cierto que es un proceso simple que seguramente es usado por muchos profesionales que acostumbran realizar este tipo de pruebas desde una máquina virtual. Sin embargo, no esta documentado para quienes esten iniciando en el mundo de las pruebas de ethical hacking
