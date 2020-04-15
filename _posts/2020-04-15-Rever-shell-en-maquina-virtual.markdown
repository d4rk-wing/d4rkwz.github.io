---
layout: post
title:  "Reverse shell en maquina virtual"
date:   2020-04-15 01:11:00 +0100
categories: Hacking
---

# Reverse shell en maquina virtual

<br />Cuando se realizan ataques de explotación de vulnerabilidades que permiten establecer una sesión sobre la maquina victima, normalmente se utilizan payload los cuales nos brindan una shell de linea de comandos de la victima sobre nuestra maquina atacante.
<br />Algunos profesionales de la seguridad suelen realizar este tipo de ataques desde una maquina virtual con las herramientas necesarias para ello (Kali, Parrot, BlackArch, etc). Esta maquina virtual suelen ejecutarla desde un equipo con sistema operativo Microsoft Windows.
<br />Dependiendo de los permisos que se tengan en la organización a la cual se le esta realizando la auditoria, un atacante podria configurar la tarjeta de red virtual en modo puente/Bridge, lo cual permitiria tomar direccionamiento propio de la compañia y facilitar el tipo de ataques.
<br />Sin embargo, en la mayoria de las compañias, se tiene direccionamiento IP Statico o reserva DCHP desde la MAC del equipo, y adicionalmente solo proporcionan una dirección IP desde la cual se ejecutan las pruebas de seguridad. Personalmente recomiendo tener el equipo con dual boot, para poder realizar las pruebas de seguridad directamente desde un sistema operativo Linux, aunque esto en algunas ocasiones no puede realizarse por limitaciones propias del cliente, en las cuales únicamente permite realizar conexiones desde sistemas operativos Windows a tavés de NAC u otros controles de seguridad.
<br />Retomando los ataques de sesión remota, al ejecutar el exploit (manual o automático) normalmente se recibe la sesión en el sistema operativo desde el cual se realiza la explotación. Por ejemplo desde Metasploit Framework, al ejecutar este tipo de ataques se establece una sesión de meterpreter o una reverse_tcp configurando la IP de la maquina atacante en la opción LHOST y especificando el puerto en LPORT. Lo anterior configura automáticamente el puerto en escucha para establecer la sesión.
<br />Pero al realizar este tipo de ataques desde una maquina virtua con configuración de red NAT, la maquina virtual adquiere una dirección IP privada la cual no es visible desde la maquina victima, por lo que al configurar esta dirección como "Local host" en el exploit, no se establece la sesión debido a que no es visible para la victima.
