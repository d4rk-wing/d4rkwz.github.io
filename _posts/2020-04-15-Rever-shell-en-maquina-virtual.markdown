---
layout: post
title:  "Reverse shell en maquina virtual"
date:   2020-04-15 01:11:00 +0100
categories: Hacking
---

# Reverse shell en maquina virtual

<br /> Cuando se realizan ataques de explotación de vulnerabilidades que permiten establecer una sesión sobre la maquina victima, normalmente se utilizan payload los cuales nos brindan una shell de linea de comandos de la victima sobre nuestra maquina atacante.
Algunos profesionales de la seguridad suelen realizar este tipo de ataques desde una maquina virtual con las herramientas necesarias para ello (Kali, Parrot, BlackArch, etc). Esta maquina virtual suelen ejecutarla desde un equipo con sistema operativo Microsoft Windows.
<br /> Dependiendo de los permisos que se tengan en la organización a la cual se le esta realizando la auditoria, un atacante podria configurar la tarjeta de red virtual en modo puente/Bridge, lo cual permitiria tomar direccionamiento propio de la compañia y facilitar el tipo de ataques.
Sin embargo, en la mayoria de las compañias, se tiene direccionamiento IP Statico o reserva DCHP desde la MAC del equipo, y adicionalmente solo proporcionan una dirección IP desde la cual se ejecutan las pruebas de seguridad. 

