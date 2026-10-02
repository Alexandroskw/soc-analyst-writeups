# TryHackMe — Nmap
**Dificultad** -> easy | **Date** -> 28-sep-26 | **Type** -> Free \
**Sala** -> [Nmap](https://tryhackme.com/room/furthernmap)
## Introducción
Análisis detallado del uso de la herramienta __Nmap__, una potente herramienta para el escaneo de redes.
## Solución
### Task 2 — Introduction
Entre más conocimiento se tenga de un objetivo, se tendrán más opciones para atacar a un sistema objetivo. Antes de realizar una audiotría de seguridad es necesario realizar un "mapeo" de la red a la que se le va a realizar dicha auditoría.

> [!NOTE]
> A un "mapeo" de red se le llama **Port scanning**.

> [!IMPORTANT]
> Cuando una computadora ejecuta un servicio de red abre un "puerto", donde recibirá la conexión. \
> Los puertos de red son necesarios para realizar múltiples peticiones o tener múltiples servicios disponibles.

> Las conexiones de red se hacen entre dos puertos
> - Puerto abierto escuchando en el servidor.
> - Puerto abierto aleatoriamente en el equipo local.

> [!TIP]
> Todas las computadoras tienen un total de **65535** puertos dispobibles.

**Nmap** se conectará a cada puerto del objeetivo y dependiendo de la respuesta se puede determinar como: **abierto**, **cerrado** o **filtrado** (usualmente por un firewall).

___
*Pregunta 1: What networking constructs are used to direct traffic to the right application on a server?*
**Respuesta: ports**

*Pregunta 2: How many of these are available on any network-enabled computer?*
**Respuesta: 65535**

*Pregunta 3: __Research__ How many of these are considered "well-known"? (These are the "standard" numbers mentioned in the task)*
**Respuesta: 1024**

> Usar la pista de la sala. \
> **NOTA**: el 0 también cuenta

### Task 3 — Nmap switches

> **Nmap** está disponible para Windows y Linux. 

___
*Pregunta 1: What is the first switch listed in the help menu for a 'Syn Scan' (more on this later!)?* \
**Respuesta: `-sS`**

> **NOTA**: Utilizar `nmap -h` en lugar de `man nmap` y buscar en `Scan techniques`

*Pregunta 2: Which switch would you use for a "UDP scan"?* \
**Respuesta: `-sU`**

*Pregunta 3: If you wanted to detect which operating system the target is running on, which switch would you use?* \
**Respuesta: `-O`**

*Pregunta 4: Nmap provides a switch to detect the version of the services running on the target. What is this switch?* \
**Respuesta: `-sV`**

*Pregunta 5: The default output provided by nmap often does not provide enough information for a pentester. How would you increase the verbosity?* \
**Respuesta: `-v`**

*Pregunta 6: Verbosity level one is good, but verbosity level two is better! How would you set the verbosity level to two?* \
**Respuesta: `-vv`**

*Pregunta 7: We should always save the output of our scans -- this means that we only need to run the scan once (reducing network traffic and thus chance of detection), and gives us a reference to use when writing reports for clients.

What switch would you use to save the nmap results in three major formats?* \
**Respuesta: `-oA`**

> **NOTA**: Recordar la `A` al final del switch

*Pregunta 8: What switch would you use to save the nmap results in a "normal" format?* \
**Respuesta: `-oN`**

> **NOTA**: Recordar la `N` al final del switch

*Pregunta 9: A very useful output format: how would you save results in a "grepable" format?* \
**Respuesta: `-oG`**

*Pregunta 10: Sometimes the results we're getting just aren't enough. If we don't care about how loud we are, we can enable "aggressive" mode. This is a shorthand switch that activates service detection, operating system detection, a traceroute and common script scanning. How would you activate this setting?* \
**Respuesta: `-A`**

*Pregunta 11: Nmap offers five levels of "timing" template. These are essentially used to increase the speed your scan runs at. Be careful though: higher speeds are noisier, and can incur errors!

How would you set the timing template to level 5?* \
**Respuesta: `-T5`**

> **NOTA**: Revisar la documentación en línea

*Pregunta 12: We can also choose which port(s) to scan.

How would you tell nmap to only scan port 80?* \
**Respuesta: `-p 80`**

> **NOTA**: No utilizar `man`, utilizar la bandera `-h` para buscar más rápido

*Pregunta 13: How would you tell nmap to scan ports 1000-1500?* \
**Respuesta: `-p 1000-1500`**

*Pregunta 14: How would you tell nmap to scan all ports?* \
**Respuesta: `-p-`**

> **IMPORTANTE**: el segundo guión "quita" las condiciones de escaneo

*Pregunta 15: How would you activate a script from the nmap scripting library (lots more on this later!)?* \
**Respuesta: `--script`**

*Pregunta 16: How would you activate all of the scripts in the "vuln" category?* \
**Respuesta: `--script=vuln`**

> **NOTA**: Revisar la documentación en línea

### Task 4 — Overview (Scan types)

| Banderas comúnes        | - `-sT` (Puertos TCP abiertos)<br>- `-sS` (Escaneos "semiabiertos" SYN)<br>- `-sU` (Escaneo UDP) |
| ----------------------- | ------------------------------------------------------------------------------------------------ |
| Banderas no tan comúnes | - `-sN` (Escaneos nulos TCP)<br>- `-sF` (Escaneos TCP FIN)<br>- `-sX` (Escaneos TCP de Navidad)  |

> [!NOTE]
> A excepción de los escaneos UDP, las demás banderas funcionan de forma muy similar pero varía la forma en la que funcionan.

___
**No se necesita respuesta**
### Task 5 — TCP Connect Scans (Scan types)
El *TCP Connect Scan* realiza un three-way handshake con cada puerto del objetivo en turno y determina si el servicio está abierto con base a la respuesta recibida.

> [!TIP]
> **Las tres fases del Three-way Handshake**
> - El cliente envía un paquete TCP con la bandera *SYN*
> - El servidor responde con las banderas *SYN/ACK*
> - El cliente responde con la bandera *ACK*

> [!IMPORTANT]
> **¿Cómo determina Nmap si un puerto está cerrado?** \
> Para determinar que un puerto está cerrado Nmap envía una petición TCP con la bandera *SYN*. El servidor objetivo entonces responderá con la bandera *RST* (Reset). Con esta última bandera Nmap establece que el puerto está cerrado.

> Muchos firewalls están configurados para descartar paquetes entrantes. Nmap envía un paquete con la bandera *SYN* y no obtiene respuesta, esto indica que el puerto está protegido por un firewall y por lo tanto el puerto se considera *filtrado*.

___
*Pregunta 1: Which RFC defines the appropriate behaviour for the TCP protocol?* \
**Respuesta: RFC 9293**

> **NOTA**: Es el estándar actual y en la tarea aparece listado

*Pregunta 2: If a port is closed, which flag should the server send back to indicate this?* \
**Respuesta: RST**

> La mayoría de las banderas son abreviaciones de su significado (SYN -> Synchronization, ACK -> Acknowledge)
## Lecciones aprendidas