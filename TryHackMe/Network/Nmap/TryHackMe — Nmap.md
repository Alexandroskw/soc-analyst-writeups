# TryHackMe — Nmap
**Dificultad** -> easy | **Date** -> 28-sep-26 | **Type** -> Free \
**Sala** -> [Nmap](https://tryhackme.com/room/furthernmap)
## Introducción
Análisis acerca del uso de la herramienta __Nmap__, una potente herramienta para el escaneo de redes.
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

### Scan types
#### Task 4 — Overview

| Banderas comúnes                                                                           | Banderas no tan comúnes                                                                   |
| ------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------- |
| `-sT` (Puertos TCP abiertos)<br>`-sS` (Escaneos "semiabiertos" SYN)<br>`-sU` (Escaneo UDP) | `-sN` (Escaneos nulos TCP)<br>`-sF` (Escaneos TCP FIN)<br>`-sX` (Escaneos TCP de Navidad) |

> [!NOTE]
> A excepción de los escaneos UDP, las demás banderas funcionan de forma muy similar pero varía la forma en la que funcionan.

___
**No se necesita respuesta**
#### Task 5 — TCP Connect Scans
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

#### Task 6 — SYN Scans

> Los *SYN scans* son utilizados para escanear un rango de puertos en el objetivo u objetivos. Se les suele llamar "Half-open" o escaneos "sigilosos" ("stealth" scans).

> [!IMPORTANT]
> A diferencia del escaneo anterior que realizaba un three-way handshake completo, este escaneo envía de vuelta una bandera RST después de recibir las banderas SYN/ACK evitando que el servidor realice la solicitud varias veces

| Ventajas                                                                                                                                                                | Desventajas                                                         |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| Se pueden eludir viejos IDS que buscan un threeway handshake completo.                                                                                                  | Requiere de permiso `sudo` para trabajar correctamente en Linux.    |
| No son registrados por las aplicaciones que están escuchando en puertos abiertos (el estándar es que se registre la conexión cuando ha sido establecida completamente). | Algunos servicios inestables pueden ser afectados por escaneos SYN. |
| Son significativamente más rápidos debido a que no se preocupan por completar el threeway handshake.                                                                    |                                                                     |

> [!WARNING]
> **Con respecto a `sudo`** \
> El usuario *root* es el único que puede crear paquetes sin procesar (raw packets) que por defecto solo él puede hacer.

> Los *SYN scans* son los escaneos por defecto en Nmap si se ejecuta con `sudo`.

> [!TIP]
> Si se escanean puertos cerrados o filtrados el comportamiento del escaneo es exactamente igual al *TCP Connect Scans*.
> - puerto cerrado: se envía una bandera RST
> - puerto filtrado: desecha el paquete (no hay respuesta) o se falsifica la bandera RST

___
*Pregunta 1: There are two other names for a SYN scan, what are they?* \
**Respuesta: Half-open, stealth**

*Pregunta 2: Can Nmap use a SYN scan without Sudo permissions (Y/N)?* \
**Respuesta: N**

>**NOTA**: Siempre se debe utilizar `sudo` si se quiere hacer un *stealth scan*.

#### Task 7 — UDP Scans
UDP a diferencia de TCP no crea una conexión. UDP envía los paquetes con la esperanza de que lleguen al puerto de destino. UDP es excelente para conexiones que requieran velocidad de conexión sobre la calidad de la misma.

> Debido a que UDP no sabe si el paquete llegó a su destino, es más difícil de escanear (además de ser más lento).

> Los puertos cerrados responden con un paquete ICMP (ping) e indiscutiblemente, el puerto UDP está cerrado

> [!NOTE]
> **¿Los puertos UDP responden?** \
> No debería de haber respuesta. Cuando esto ocurre, Nmap marca el puerto como `open|filtered`, es decir, puede estar abierto pero detrás de un firewall. \
> Si hay respuesta (que es extremadamente raro), se marca como abierto y se envía la solicitud una segunda vez, si no hay respuesta se vuelve al estado `open|filtered`.

> [!TIP]
> Es buena práctica ejecutar un escaneo con la opción `--top-ports <número>` para disminuír el tiempo de escaneo (en comparación, un escaneo TDP se puede ejecutar en ~20 minutos en los primeros 1000 puertos).

___
*Pregunta 1: If a UDP port doesn't respond to an Nmap scan, what will it be marked as?* \
**Respuesta: `open|filtered`**

*Pregunta 2: When a UDP port is closed, by convention the target should send back a "port unreachable" message. Which protocol would it use to do so?* \

#### Task 8 — NULL, FIN and Xmas
No son tan populares y son extremadamente raros de utilizar principalmente porque son mucho más sigilosos que un *SYN Scan*.

| Escaneo  | ¿Qué hace?                                                                                                                                                                                                 | Bandera en Nmap |
| :------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------: |
| **NULL** | La petición TCP se envía sin ninguna bandera. Según el **RFC** la víctima debería responder con un RST si el puerto está cerrado.                                                                          |      `-sN`      |
| **FIN**  | Funciona similar, pero en lugar de enviar un paquete completamente vacío, manda la bandera FIN que se utiliza para cerrar ordenadamente la conexión activa y una vez más la víctima responderá con un RST. |      `-sF`      |
| **Xmas** | Manda un paquete TCP "malformado". Espera una respuesta RST para los puertos cerrados.                                                                                                                     |      `-sX`      |

> [!TIP]
> **¡Luces navideñas!** \
> El escaneo de navidad (*Xmas scan*) envía un paquete con las banderas FIN, PSH y URG encendidas al mismo tiempo lo que hace que el paquete quede "iluminado" como un árbol de navidad.

> [!WARNING]
> **Ya sé como vas a reaccionar** \
> Al igual que en *UDP scan*, se espera este comportamiento si el puerto se protege con un firewall. Por lo tanto solo identificarán los puertos como: *open|filtered*, *closed* o *filtered*.

> Si el puerto se marca como filtrado, el puerto ha respondido con un paquete ICMP.

> [!TIP]
> RFC 793 dice que los host deben responder a paquetes malformados con la bandera RST en los puertos cerrados y no responder para los abiertos, Microsoft y Cisco no siguen esta regla. Ellos responden RST a cualquier caso haciendo que los puertos aparezcan como cerrados.

> El objetivo de estos escaneos es evadir el firewall

___
*Pregunta 1: Which of the three shown scan types uses the URG flag?* \
**Respuesta: xmas**

*Pregunta 2: Why are NULL, FIN and Xmas scans generally used?* \
**Respuesta: firewall evasion**

> **RECORDATORIO**: Muchos IDS modernos saben cómo funciona el *SYN Scan* tradicional

*Pregunta 3: Which common OS may respond to a NULL, FIN or Xmas scan with a RST for every port?* \
**Respuesta: Microsoft Windows**

> **NOTA**: tanto Microsoft como Cisco no siguen el **RFC 793**.

#### Task 9 — ICMP Network Scanning
Al conectarse por primera vez a una red objetivo, el primer paso es obtener el "mapa" de la red, es decir, qué direcciones IP tienen hosts activos y cuales no.

> Una forma de realizar este mapeo es con "ping sweep" (barrido de ping).

Nmap envía un paquete ICMP a cada una de las IP dentro de la red. Cuando la IP responde se marca como "viva".

> [!IMPORTANT]
> Cuando se hace un ping sweep no es del todo exacto marcar como "viva" a una IP. Puede proveer un poco de contexto, es importante remarcarlo.

Para realizar un ping sweep se utiliza la bandera `-sn` con el rango de IP
- Rango especificado con guión: `nmap -sn 192.168.0.1-254`
- Rango especificado con CIDR `nmap -sn 192.168.0.0/24`

La bandera `-sn` obliga a Nmap a no escanear ningún puerto y depender de paquetes ICMP para identificar a los objetivos.

> [!WARNING]
> Nmap además de enviar el paquete ICMP, también enviará un paquete SYN al puerto 443 del objetivo junto con un paquete ACK (o SYN si no es *root*) al puerto 80.

___
*Pregunta 1: How would you perform a ping sweep on the 172.16.x.x network (Netmask: 255.255.0.0) using Nmap? (CIDR notation)* \
**Respuesta: `nmap -sn 172.16.0.0/16`**

### NSE Scripts
#### Task 10 — Overview

> NSE = **N**map **S**cripting **E**ngine


## Lecciones aprendidas
