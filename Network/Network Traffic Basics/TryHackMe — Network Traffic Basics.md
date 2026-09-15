**Difficult** -> easy | **Date** -> 24-jul-26 | **Type** -> Free
**Room** -> [Network Traffic Basics](https://www.tryhackme.com/room/networktrafficbasics)
## Introduction
El Análisis de Trafico de Red (**NTA**) es un proceso que abarca la captura, inspección y el análisis de los datos que fluyen en una red y su principal objetivo es tener la visibilidad completa de lo que entra y sale de la red. El **NTA** no es sinónimo de **Wireshark**.
El NTA va más allá, es una combinación de correlación de varios registros, inspección profunda de paquetes y estadísticas de flujo de red. Es una habilidad esencial para cualquier analista SOC L1 y otros roles en Blue y Red team. Como analista L1, debes ser hábil al navegar en un mar de datos de red y entender qué es normal y qué se sale de la línea base.
## Solution
### Task 2 — ¿Cuál es el propósito del Análisis de Tráfico de red?
Basado en los logs DNS podemos obtener la siguiente información:
- Consulta y tipo de consulta
- Subdominio y TLD: podemos usar herramientas como **abuseDB** o **VirusTotal** para verificar si el dominio es malicioso
- Host IP: podemos identificar al sistema enviando las consultas DNS
- IP de destino: podemos usar herramientas como AbuseIPDB o VirusTotal para verificar si la IP está marcada como maliciosa
- Timestamp: podemos construir una línea de tiempo mapeando las diferentes consultas maliciosas
Los logs DNS no contienen más información que esta, así que es difícil hacer una conclusión basado solo en esta información. Se necesita inspeccionar mas en profundidad el tráfico DNS y verificar el contenido de la consulta y la respuesta.
Esto es conocido como DNS Tunneling.
Los firewalls y otros dispositivos registran las consultas de DNS pero no el contenido de las mismas. Debemos de investigar más en profundidad el contenido de las consultas y las respuestas ya que esto nos permitirá determinar la naturaleza de esas consultas y respuesatas.
#### ¿Por qué se debe analizar el tráfico de red?
Desde la perspectiva de un analista SOC:
- Detectar actividad maliciosa o sospechosa
- Reconstruir ataques durante una respuesta a incidente
- Verificar y validar alertas
*Pregunta 1: ¿Cuál es el nombre de la técnica utilizada para contrabandear comandos C2 vía DNS?*
**Respuesta: DNS Tunneling**
### Task 3 — ¿Qué tráfico de red podemos observar?
La mejor manera que tenemos de mostrar el tráfico de red es utilizando la arquitectura implementada en cualquier dispositivo con una interfaz de red: la pila TCP/IP. Cada capa describe la información requerida, llamada **header** (encabezado) para pasar los datos al siguiente nivel de la pila.
Lo que transportan estos headers junto con los datos de aplicación, es precisamente lo que queremos observar ya que los logs usualmente incluyen bits y piezas de los headers pero nunca los detalles completos del paquete.
#### Application
En la capa de aplicación podemos encontrar dos estructuras de información importantes:
- la información del header de la aplicación
- los datos de la aplicación en si (payload)

Esta información cambiará dependiendo de qué protocolo de la capa de aplicación estemos utilizando, `HTTP` o `HTTPS`.
El código abajo muestra los headers de aplicación de un cliente enviando una petición `GET` y el servidor respondiendo, muchos web proxies y firewalls registran estos datos del header sin embargo, lo que no registran son los datos de aplicación o el payload. 
*Petición:*
```json
GET /downloads/suspicious_package.zip HTTP/1.1
Host: www.tryhackrne.thn
User-Agent: curl/7.85.0
Accept: */*
Connection: close
```
De la petición `GET` se puede determinar que el cliente está pidiendo un archivo de nombre `suspicious_package.zip` y el servidor responde con un código 200 que significa que ha aceptado la petición. Pero no podemos ver el contenido del archivo, es decir, la última línea. Dicho de otra manera, es imposible ver qué es lo que hay comprimido dentro del `.zip`.
*Respuesta:*
```json
HTTP/1.1 200 OK
Date: Mon, 29 Sep 2025 10:15:30 GMT
Server: nginx/1.18.0
Content-Type: application/zip
Content-Length: 10485760
Content-Disposition: attachment; filename="suspicious_package.zip"
Last-Modified: Mon, 29 Sep 2025 09:54:00 GMT
ETag: "5d8c72-9f8a1c-3a2b4c"
Accept-Ranges: bytes
Connection: close

[binary ZIP file bytes follow — 10,485,760 bytes]
```
#### Transport
Los datos de la aplicación y el header son segmentados y encapsulados en piezas más pequeñas y cada pieza incluye un header de transporte, en muchos casos es `TCP` o `UDP`. Los logs del firewall usualmente incluyen los puertos de origen y destino así como las banderas pero no los demás campos. Sin embargo, con estos campos son invaluables para detectar cierto tipo de ataques como un **session hijacking** (secuestro de sesión).
Los session hijacking se pueden detectar analizando el campo de secuencia (`Seq`) del header; si es muy grande de repente, se requiere una investigación.
```json
No.     Time        Source          Destination     Protocol Length  Info
1       0.000000    192.168.1.45    172.217.22.14   TCP      74      51432 → 80 [SYN] Seq=0 Win=64240 Len=0 MSS=1460
2       0.000120    172.217.22.14   192.168.1.45    TCP      74      80 → 51432 [SYN, ACK] Seq=0 Ack=1 Win=65535 Len=0 MSS=1460
3       0.000220    192.168.1.45    172.217.22.14   TCP      66      51432 → 80 [ACK] Seq=1 Ack=1 Win=64240 Len=0
4       0.010500    192.168.1.45    172.217.22.14   TCP      1514    51432 → 80 [PSH, ACK] Seq=1 Ack=1 Win=64240 Len=1460
5       0.010620    172.217.22.14   192.168.1.45    TCP      66      80 → 51432 [ACK] Seq=1 Ack=1461 Win=65535 Len=0
6       0.020100    192.168.99.200  172.217.22.14   TCP      74      51432 → 80 [PSH, ACK] Seq=34567232 Ack=1 Win=64240 Len=20
```
- Las primeras 3 líneas muestran un 3-way handshake TCP legítimo
- Las líneas 4 y 5 muestran una transferencia de datos legítima
- La línea 6 muestra un paquete tratando de inyectarse a si mismo en la sesión, hay que notar el salto masivo en la secuencia de números.
#### Internet
Cuando la capa de transporte envía un segmento, la capa de internet agrega su propio header. Si el segmento es más grande que el MTU (Maximum Transmission Unit), se dividirá en fragmentos a los cuales se les agregará también el header de Internet. Los campos que se registran usualmente en esta capa son la dirección IP de origen y destino así como el TTL.
En la mayoría de escenarios es más que suficiente pero si por ejemplo nos encontramos un **fragmentation attack**, necesitamos revisar el offset del fragmento y la longitud total. Un atacante podría crear fragmentos más pequeños para evadir el IDS o corromper el reensamblaje de los fragmentos utilizando rangos de bytes superpuestos, por porner un ejemplo.
En el ejemplo de abajo se puede apreciar esta técnica; el offset en la línea 3 se sobrepone con el de la línea 2, lo que significa que el reensamble del paquete se puede realizar con uno o con otro haciendo un bypass del IDS.
```json
No.   Time       Source        Destination   Protocol Length Info
1     0.000000   203.0.113.45  192.168.1.10  UDP      1514    Fragmented IP protocol (UDP) (id=0x1a2b) [MF] Offset=0, Len=1480
2     0.000015   203.0.113.45  192.168.1.10  UDP      1514    Fragmented IP protocol (UDP) (id=0x1a2b) [MF] Offset=1480, Len=1480
3     0.000030   203.0.113.45  192.168.1.10  UDP       600    Fragmented IP protocol (UDP) (id=0x1a2b) Offset=1480, Len=64   <-- Overlap
4     0.000045   192.168.1.10  203.0.113.45  ICMP      98     Destination unreachable (Fragment reassembly time exceeded)
```
#### Enlace
Una vez que la capa de Internet termina su encapsulación, el paquete IP pasa a la capa de enlace y también agrega su propio header que contiene más información de direccionamiento. Muchos logs desplegarán la dirección MAC de origen y destino, para cierto tipo de ataques como **ARP poisoning** o **spoofing** no es suficiente, se necesita el paquete completo.
No se puede ver los logs es cuando la dirección MAC aparece desde múltiples interfaces o cuando se envía muchos paquetes ARP gratuitos con direcciones MAC conflictivas.
```json
No.   Time       Source           Destination      Protocol Length Info
1     0.000000   192.168.1.1      Broadcast        ARP      60     Who has 192.168.1.10? Tell 192.168.1.1
2     0.000025   192.168.1.10     192.168.1.1      ARP      60     192.168.1.10 is at 00:11:22:33:44:55
3     1.002010   192.168.1.200    192.168.1.1      ARP      60     192.168.1.10 is at aa:bb:cc:dd:ee:ff  <-- Attacker spoof
4     1.002015   192.168.1.200    192.168.1.10     ARP      60     192.168.1.1 is at aa:bb:cc:dd:ee:ff  <-- Attacker spoof
5     1.100000   192.168.1.10     172.217.22.14    TCP      74     54433 → 80 [SYN] Seq=0 Win=64240 Len=0
6     1.100120   192.168.1.200    172.217.22.14    TCP      74     54433 → 80 [SYN] Seq=0 Win=64240 Len=0  <-- Relayed via attacker
```
El ejemplo de arriba muestra una captura de paquetes que detalla un ataque de ARP poisoning. El host con la dirección IP `192.168.1.200` está respondiendo cada petición ARP con la misma MAC.

---
*Pregunta 1: Look at the HTTP example in the task and answer the following question: What is the size of the ZIP attachment included in the HTTP response? Note down the answer in bytes.*
**Respuesta: 10485760**
*Tip: Hay que fijarse en el resaltado amarillo de la primera capa del modelo TCP/IP*

*Pregunta 2 Which attack do attackers use to try to evade an IDS?*
**Respuesta: fragmentation**
*Tip: Lo que hace es hacer fragmentos más pequeños para evadir el MTU y por lo tanto evadir el IDS*

*Pregunta 3: What field in the TCP header can we use to detect session hijacking?*
**Respuesta: sequence number**
*Tip: Si la secuencia de números es muy grande, quiere decir que hay un intento de hijacking*

### Task 4 — Nerwork traffic sources and flows
En una red corporativa típica, tiene algunas fuentes y flujos de red predeterminados, es más útil enfocarnos en flujos y fuentes específicas. Las fuentes se pueden agrupar en dos categorías
- Intermediario
- Endpoints
A su vez, los flujos también se pueden agrupar en dos categorías
- North-South: Tráfico que existe o entra en la LAN y pasa el firewall
- East-West: Tráfico que se queda en la LAN (incluyendo la LAN que se extiende a la nube)
#### Intermediary Sources
Son los dispositivos por donde pasa la mayoría del tráfico de red. El tráfico que generan es significativamente más bajo que un endpoint. En esta categoría se encuentran: firewalls, switches, web proxies, IDS, IPS, routers, access points, controladores wireless de LAN (Wi-Fi), entre otros.
El tráfico que originan viene de servicios como protocolos de enrutamiento (EIGRP, OSPF, BGP), protocolos de administración (SNMP, PING), protocolos de acceso (SYSLOG) y otros protocolos de soporte (ARP, STP, DHCP).
#### Endpoints Sources
Es donde la gran parte del tráfico se origina y termina. Estos dispositivos consumen la mayoría del ancho de banda de la red. Los dispositivos que caen en esta categoría son: servidores, hosts, dispositivos IoT, impresoras, máquinas de laboratorio, recursos de la nube, celulares, tablets, entre otros.
#### North-South Traffic
Se monitorea de cerca a medida que fluye desde la LAN a la WAN y viceversa. Los servicios que caen en esta categoría son protocolos cliente-servidor como HTTPS, DNS, SSH, SMTP, VPN, RDP entre otros. Todo el tráfico que generan estos protocolos pasa a través del firewall de una manera u otra.
#### East-West Traffic
Se queda dentro de la LAN corporativa, así que es menos monitoreada. Sin embargo, es importante mantenerla monitoreada ya que cuando la red es comprometida, un atacante podría explotar diferentes servicios para moverse lateralmente en la red.
Algunos servicios de esta categoría son
- Directory, Authentication & Identity Services
	- Kerberos / LDAP: Authentication/queries to Active Directory
	- RADIUS / TACACS+: Network access control
	- Certificate Authority issuing internal certifications
- File shares & print services
	- SMB/CIFS: Accessing network drives
	- IPP/LPD: Printing over the network
- Router, switching, and infrastructure services
	- DHCP traffic between hosts and the DHCP server
	- ARP broadcast messages
	- Internal DNS
	- Routing protocol messages
- Application Communication
	- Database Connections: SQL over TCP
	- Microservices APIs: REST or gRPC calls between services
- Backup & Replication
	- File Replication: Between data centers or to backup servers
	- Database Replication: MySQL binlog replication, PostgreSQL streaming, and more
- Monitoring & Management
	- SNMP: Device health metrics
	- Syslog: Centralized logging
	- NetFlow/IPFIX: Traffic flow telemetry
	- Other endpoint logs sent to a central logging server
#### Flow examples
#### HTTPS
#### HTTPS
Un host pide un sitio web, esta respuesta es enviada al NGFW (Next Generation Firewall) que incluye un web proxy. El proxy actua como el servidor y establece una conexión TCP con el servidor web original y redirige las peticiones de los clientes. Cuando el proxy recibe la respuesta inspecciona el contenido y lo envía al host que lo ha solicitado y lo encuentra seguro.
![[Pasted image 20260727143115.png]]
#### DNS Externo
El tráfico DNS con una red corporativa empieza cuando el host envía una query DNS a través del puerto 53 que actúa en nombre del host. Primero revisa si tiene la respuesta a la petición en la caché, si no la tiene envía una petición vía róuter a través del firewall a los DNS configurados.
![[Pasted image 20260727143340.png]]
### Task 5 — How we can observe network traffic?
Podemos obtener información para el análisis de tráfico de red de varias formas
- Logs
- Captura completa de paquetes
- Estadísticas de red
#### Logs
Es la primera "línea de defensa" para el análisis de lo que está ocurriendo en la red. Es importante mencionar que no existe un estándar para implementar logging en cada sistema y protocolo. Cuando los registros no proveen suficiente información, es necesario relacionar los registros, inspeccionar las capturas de paquetes y revisar las estadísticas de red
#### Full packet capture
Hay dos formas de capturar los paquetes completos
- Instalar un TAP de red físico
- Configurar un port mirroring
#### Network TAP
Es un dispositivo físico que se pone en la red para capturar todo el tráfico que pasa sin afectar el desempeño. Los los datos entonces se envían a un IDS, packet capture box u otro tipo de sistema dedicado. El TAP únicamente opera en la capa de enlace del modelo TCP/IP. No necesita la dirección MAC o IP porque copia las señales eléctricas y las envía al puerto de monitoreo.
#### Port mirroring
Es un software que copia los paquetes de un dispositivo y los manda a otro dentro de la red para monitorearlos, por ejemplo un IDS u otros sistemas. Cada fabricante lo llama de una manera, en Cisco por ejemplo se le conoce como **SPAN**.
![[Pasted image 20260727150959.png]]
#### Best practices
Cuando se vaya a hacer una captura de paquetes, es necesario tener en cuenta
- Posición: dependiendo de qué tráfico queremos capturar, necesitamos posicionar correctamente el TAP o configurar el mirror correctamente.
- Duración: la captura completa de paquetes necesita una cantidad de almacenamiento considerable. Si se captura 1Gbps de un día entero, posiblemente necesitemos 10.8 TB de almacenamiento.
- Mirror vs. TAP: los TAP's ofrecen una reducción del desempeño casi nula. El mirroring puede agregar latencia cuando una gran cantidad de tráfico está ocurriendo a través del puerto de mirroring.
#### Actividad
El ejercicio 1 pide posicionar un dispositivo TAP en el sitio correcto ya que un usuario utilizando una workstation aleatoria, ha hecho clic en un link de phishing y ha iniciado una petición HTTPS y la descarga de un archvio de PowerShell malicioso.
![[Pasted image 20260727152331.png]]

Lo lógico sería poner el TAP antes del **SW02** pero estaríamos consumiendo mucho almacenamiento al capturar los paquetes de todas las workstations. En el **SW01** tampoco puede ser puesto ya que consumiría en exceso almacenamiento por el Servidor DNS y el servidor de correo. Ponerlo en frente de las workstation es completamente inútil; no sabemos cuál fue utilizada.
Si vamos para atrás, podemos ver Web Proxy (**WP01**). Como mencionan las instrucciones, se hizo una petición HTTP. Todos los dispositivos en la red hacen una petición HTTP(S) en este dispositivo ya que es el intermediario con el sitio real. Ahora podemos analizar el tráfico entrante y saliente
![[Pasted image 20260727155854.png]]
Ahora podemos empezar a buscar la bandera. Recordemos que el usuario hizo clic en un link malicioso en un correo de phishing y empezó a descargar un archivo. Esto último, la descarga es nuestra pista. En el análisis de los paquetes a la izquierda, podemos ver la información de los paquetes. Cuando se hace una conexión TCP correcta, se manda un código **200 OK**, que ha establecido conexión exitosamente.
Con esto en mente podemos empezar a buscar con los código 200 que veamos. Sin embargo, debemos tomar en cuenta que está descargando un archivo, eso reduce aun más la búsqueda.
El paquete que nos interesa está en la última página y al abrirlo aparece la bandera
![[Pasted image 20260727161202.png]]
**Bandera: THM{FoundTheMalware}**
El ejercicio 2 dice que una workstation ha sido comprometida e instrucciones C2 (Command & Control) están siendo ejecutadas por registros DNS de texto. Necesitamos también capturar el tráfico DNS de la red.
Lo importante aquí es el DNS, y en el ejercicio anterior, ya habíamos visto un servidor DNS. Lo lógico es poner el TAP en el servidor DNS y capturar todo su tráfico.
![[Pasted image 20260727162010.png]]
Como mencionan las instrucciones, se están inyectando comandos C2 por medio de un archivo de texto (TXT) en el DNS. Es importante recordar que el tráfico DNS va a través del puerto 53 y que estamos buscando C2 y un TXT. Con esto en mente, me puse a investigar en las descripciones de los paquetes si había una descripción con TXT o C2. En la segunda página está lo que estaba buscando. La información del paquete dice:
```json
Standard query response 0x41eb TXT c2.tryhackrne.thn
```
Contiene un TXT y es un C2. Si abrimos el paquete en la sección de **query** dice que es un c2 por medio de un TXT (TXT/IN)
![[Pasted image 20260727164305.png]]
Junto a la respuesta podemos encontrar la bandera **THM{C2CommandFound}**
Completando el ejercicio.
#### Tools
Hay algunas herramientas especializadas para capturar estos paquetes
- Wireshark
- TCPdump
- IPS/IDS como snort, suricata y zeek

## References
[[TryHackMe — Comando dig]]
[[TryHackMe — Comando WHOIS]]
[[TryHackMe — Modelo TCP-IP]]