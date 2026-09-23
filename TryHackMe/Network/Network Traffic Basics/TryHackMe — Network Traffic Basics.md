# TryHackMe — Network Traffic Basics
**Dificultad** -> easy | **Date** -> 24-jul-26 | **Type** -> Free + Hands-On \
**Sala** -> [Network Traffic Basics](https://www.tryhackme.com/room/networktrafficbasics)

# Introducción
Sala enfocada en entender el análisis de red (NTA) y qué herramientas se utilizan para recolectar el tráfico.

# Solución
### Task 2 — What is the Purpose of Network Traffic Analysis?
Gracias a los logs de DNS se puede obtener
- Peticiones y tipo de peticiones
- **Subdominio** y **TLD**: se pueden usar herramientas como **abuseDB** o **VirusTotal** para verificar si un dominio es malicioso
- **Host IP**: se puede identificar un sistema enviando las consultas DNS
- **IP de destino**: se pueden usar herramientas como [AbuseIPDB](https://www.abuseipdb.com/) o [VirusTotal](https://www.virustotal.com/gui/home/upload) para verificar si la IP está marcada como maliciosa
- **Timestamp**: se construir una línea de tiempo mapeando las diferentes consultas maliciosas

> [!NOTE]
> Los logs de DNS solo contienen esta información, por lo que no se puede hacer un análisis solo con esta información.

Se debe inspeccionar con mas profundidad el tráfico DNS y verificar el contenido de la consulta y la respuesta.

> [!WARNING]
> Los firewalls y otros dispositivos registran las consultas de DNS pero no el contenido de las mismas.

> Investigar más en profundidad el contenido de las consultas y las respuestas. Ya que esto permitirá determinar la naturaleza de las consultas y respuesatas.

#### ¿Por qué se debe analizar el tráfico de red?

| De forma general                                                                     | Desde la perspectiva del SOC                           |
| ------------------------------------------------------------------------------------ | ------------------------------------------------------ |
| Monitorear el desempeño de red                                                       | Detectar actividad maliciosa o sospechosa              |
| Revisar anormalidades en la red                                                      | Reconstruir ataques durante una respuesta a incidentes |
| Revisa el contenido de las comunicaciones sospechosas, tanto internas como externas. | Verificar y validar alertas                            |

> [!TIP]
> El **DNS Tunneling** consiste en la filtración de datos a través de las peticiones o tráfico DNS.

___
*Pregunta 1: What is the name of the technique used to smuggle C2 commands via DNS?* \
**Respuesta: DNS Tunneling**

### Task 3 — ¿Qué tráfico de red podemos observar?
Se puede observar el tráfico de red a través del modelo TCP/IP, ya que cada capa agrega su propio **header** para posteriormente pasar los datos a la siguiente capa inmediatamente inferior.

> [!IMPORTANT]
> Los logs suelen incluir fragmentos de los headers pero nunca detalles completos del paquete.

#### Application
En la capa de aplicación podemos encontrar dos estructuras de información importantes: la información del header de la aplicación y los datos de la aplicación en si (la payload).

> La información cambia dependiendo de qué protocolo de la capa de aplicación estemos utilizando (`HTTP` o `HTTPS`).

```json
// Header de la aplicación enviando una petición 'GET'.
GET /downloads/suspicious_package.zip HTTP/1.1
Host: www.tryhackrne.thn
User-Agent: curl/7.85.0
Accept: */*
Connection: close
```

El cliente hace la petición a un archivo de nombre `suspicious_package.zip` y el servidor responde con `200 OK`, la petición fue aceptada.

````json
// Servidor respondiendo a la petición
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

```
[binary ZIP file bytes follow — 10,485,760 bytes]
```
````

No se puede ver el contenido del archivo, es decir, es imposible ver qué es lo que hay comprimido dentro del `.zip`, la última línea del bloque anterior.

#### Transport
Los datos de la aplicación y el header son segmentados y encapsulados en piezas más pequeñas. Cada pieza incluye un header de transporte, en muchos casos es `TCP` o `UDP`.

> Los logs del firewall usualmente incluyen los puertos de origen, destino y las banderas pero no otros campos.

> [!IMPORTANT]
> Los campos de puerto de origen y destino son invaluables para detectar un tipo de ataque en concreto llamado **session hijacking** (secuestro de sesión).


```json
No.     Time        Source          Destination     Protocol Length  Info
1       0.000000    192.168.1.45    172.217.22.14   TCP      74      51432 → 80 [SYN] Seq=0 Win=64240 Len=0 MSS=1460
2       0.000120    172.217.22.14   192.168.1.45    TCP      74      80 → 51432 [SYN, ACK] Seq=0 Ack=1 Win=65535 Len=0 MSS=1460
3       0.000220    192.168.1.45    172.217.22.14   TCP      66      51432 → 80 [ACK] Seq=1 Ack=1 Win=64240 Len=0
4       0.010500    192.168.1.45    172.217.22.14   TCP      1514    51432 → 80 [PSH, ACK] Seq=1 Ack=1 Win=64240 Len=1460
5       0.010620    172.217.22.14   192.168.1.45    TCP      66      80 → 51432 [ACK] Seq=1 Ack=1461 Win=65535 Len=0
6       0.020100    192.168.99.200  172.217.22.14   TCP      74      51432 → 80 [PSH, ACK] Seq=34567232 Ack=1 Win=64240 Len=20
```

Las tres primeras líneas muestran un **Three-Way handshake** legítimo, mientras que las líneas 4 y 5 son un intercambio legítimo. La última línea muestra un paquete de otra fuente tratando de inyectarse en la sesión, hay que prestar atención en el salto masivo en la secuencia numérica.

> [!WARNING]
> Las **session hijacking** se pueden detectar analizando el campo de secuencia (`Seq`) en la sección `Info` del header. Si crece muy bruscamente, requiere una investigación más en profundidad.

#### Internet
La capa de internet agrega su propio header una vez que la capa de transporte le manda el paquete.

> [!NOTE]
> Si el segmento es más grande que el **MTU** (Maximum Transmission Unit), se dividirá en fragmentos a los cuales se les agregará también el header de Internet.

Los campos que se registran en esta capa son usualmente la dirección IP de origen, destino y el TTL.

> [!IMPORTANT]
> **Fragmentation attack** \
> Ocurre cuando el atacante manipula la fragmentación de los paquetes IP para hace un bypass a los controles de seguridad o interrumpir el funcionamiento de los sistemas.


```json
// Offset de la línea 3 se sobrepone con el de la línea 2. El reensamble del paquete se puede realizar con cualquiera de los dos. Bypass al IDS es posible
No.   Time       Source        Destination   Protocol Length Info
1     0.000000   203.0.113.45  192.168.1.10  UDP      1514    Fragmented IP protocol (UDP) (id=0x1a2b) [MF] Offset=0, Len=1480
2     0.000015   203.0.113.45  192.168.1.10  UDP      1514    Fragmented IP protocol (UDP) (id=0x1a2b) [MF] Offset=1480, Len=1480
3     0.000030   203.0.113.45  192.168.1.10  UDP       600    Fragmented IP protocol (UDP) (id=0x1a2b) Offset=1480, Len=64   <-- Overlap
4     0.000045   192.168.1.10  203.0.113.45  ICMP      98     Destination unreachable (Fragment reassembly time exceeded)
```

> Para verificar un **fragmentation attack** se necesita revisar el offset del fragmento y la longitud total ya que se puede reensamblar los fragmentos utilizando rangos superpuestos.

#### Data Link
El paquete IP pasa a la capa de enlace y también agrega su propio header. Algunos logs despliegan la dirección MAC de origen y destino, que para ataques tipo **ARP poisoning** o **spoofing** por si sola no es suficiente. \
No se puede ver los logs cuando la dirección MAC aparece desde múltiples interfaces o cuando se envía muchos paquetes ARP gratuitos con direcciones MAC conflictivas.

```json
// El host responde cada petición ARP con la misma dirección MAC.
// A esta práctica se le conoce como ataque ARP poisoning.
No.   Time       Source           Destination      Protocol Length Info
1     0.000000   192.168.1.1      Broadcast        ARP      60     Who has 192.168.1.10? Tell 192.168.1.1
2     0.000025   192.168.1.10     192.168.1.1      ARP      60     192.168.1.10 is at 00:11:22:33:44:55
3     1.002010   192.168.1.200    192.168.1.1      ARP      60     192.168.1.10 is at aa:bb:cc:dd:ee:ff  <-- Attacker spoof
4     1.002015   192.168.1.200    192.168.1.10     ARP      60     192.168.1.1 is at aa:bb:cc:dd:ee:ff  <-- Attacker spoof
5     1.100000   192.168.1.10     172.217.22.14    TCP      74     54433 → 80 [SYN] Seq=0 Win=64240 Len=0
6     1.100120   192.168.1.200    172.217.22.14    TCP      74     54433 → 80 [SYN] Seq=0 Win=64240 Len=0  <-- Relayed via attacker
```

---
*Pregunta 1: Look at the HTTP example in the task and answer the following question: What is the size of the ZIP attachment included in the HTTP response? Note down the answer in bytes.* \
**Respuesta: 10485760**

> **TIP**: Revisar el resaltado amarillo en la primer capa del modelo TCP/IP

*Pregunta 2 Which attack do attackers use to try to evade an IDS?* \
**Respuesta: fragmentation**

> **PALABRAS CLAVE** -> *evade an IDS*

*Pregunta 3: What field in the TCP header can we use to detect session hijacking?* \
**Respuesta: sequence number**

> **TIP**: Revisar la secuencia de números (`Seq`) en la sección `Info`.

### Task 4 — Nerwork traffic sources and flows
#### Sources

| Tipo de fuente | Enfoque                                                                                            | ¿Quiénes son?                                                                                            |
| -------------- | -------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| Intermediarios | Dispositivos por donde pasa la mayoría del tráfico de red.                                         | Firewalls, switches, proxies, IDS/IPS, róuters, access points, Wi-Fi, entre otros.                       |
| Endpoints      | Dispositivos que generan gran parte del tráfico. Consumen la mayoría del ancho de banda de la red. | Servidores, hosts, dispositivos IoT, impresoras, recursos de la nube, dispositivos móviles, entre otros. |

#### Flows 
| Tipos de flujos | ¿En qué área de la red estan?                                            | ¿Quién los genera?                                                                                                                                                                                                           |
| :-------------: | ------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|   North-South   | Tráfico que sale o entra en la LAN y pasa el firewall                    | HTTPS, DNS, SSH, SMTP, VPN, RDP <br>(entre otros)                                                                                                                                                                            |
|    East-West    | Tráfico que se queda en la LAN. Incluye la LAN que se extiende a la nube | - Directory, Authentication & Identity Services <br>- File shares & print services<br>- Router, switching, and infrastructure services<br>- Application Communication<br>- Backup & Replication<br>- Monitoring & Management |

#### Flow examples

|       Flujo       | ¿Qué hace?                                                                                                                                                                                                                                                                              |
| :---------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|       HTTPS       | Se tiene dos sesiones: una entre el cliente y el proxy, y otra entre el proxy y el servidor web. Desde el punto de vista del cliente, este ha establecido una sesión con el servidor web.                                                                                               |
|   External DNS    | El host envía la consulta al servidor DNS interno en el puerto 53, el cual actuará en nombre del host. Verificará si tiene una respuesta a la consulta en su caché. De no ser así enviará la consulta a través del enrutador pasando por el firewall a los servidores DNS configurados. |
| SMB with Kerberos | Al abrir un recurso compartido se crea una sesión SMB.<br>El equipo usa Kerberos: con el Ticket Granting Ticket (TGT) ya obtenido, pide un ticket de servicio y lo usa para conectarse y acceder al recurso.                                                                            |
___
*Pregunta 1: Which category of devices generates the most traffic in a network?* \
**Respuesta: endpoint**

*Pregunta 2: Before an SMB session can be established, which service needs to be contacted first for authentication?* \
**Respuesta: kerberos**

> **NOTA**: La sesión **SMB** se debe crear cuando se abre algún recurso compartido.

*Pregunta 3: What does TLS stand for?* \
**Respuesta: Transport Layer Security**

> **RECORDATORIO**: Se relaciona con el protocolo HTTPS.

### Task 5 — How we can observe network traffic?
Se puede obtener información para el análisis de tráfico de red de varias formas: **Logs**, **full packet capture** y **network statistics**.

| Fuente              | ¿Qué sucede?                                                                                                                                                                                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Logs                | Es la primer línea de defensa de lo que ocurre en la red. Los datos que se registran dependen del proveedor.                                                                                                                                                                                                                         |
| Full packet capture | - **TAP de red físico**: dispositivo físico que se pone en la red para capturar todo el tráfico y posteriormente se envía a un IDS o algún otro sistema dedicado.<br>- **Port mirroring**: es un software que copia los paquetes de un dispositivo y lo envía a otro dentro de la misma red para monitorearlos, por ejemplo, un IDS. |
| Network statistics  | - **NetFlow**: es un protocolo desarrollado por Cisco que recopila metadatos sobre el tráfico que circula por una red.<br>- **IPFIX**: En colaboración con Cisco y otros proveedores, el IETF creó IPFIX y lo publicó como un estándar independiente de proveedores.                                                                 |

> [!NOTE]
> **Acerca del TAP** \
> El TAP opera en la capa de enlace. No necesita la dirección MAC o IP porque copia las señales eléctricas y las envía al puerto de monitoreo.

#### Best practices
Cuando se vaya a hacer una captura de paquetes, es necesario tener en cuenta
- **Posición**: dependiendo de qué tráfico queremos capturar, necesitamos posicionar correctamente el TAP o configurar el mirror correctamente.
- **Duración**: la captura completa de paquetes necesita una cantidad de almacenamiento considerable. Si se captura 1Gbps de un día entero, posiblemente necesitemos 10.8 TB de almacenamiento.
- **Mirror** vs. **TAP**: los TAP's ofrecen una reducción del desempeño casi nula. El mirroring puede agregar latencia cuando una gran cantidad de tráfico está ocurriendo a través del puerto de mirroring.

___
*Pregunta 1: What is the flag found in the HTTP traffic in scenario 1? The flag has the format THM{}.* \
**Respuesta: THM{FoundTheMalware}**

> Todos los dispositivos en la red hacen una petición HTTP en proxy (**WP1**) ya que es el intermediario con el sitio real.

![tap_after](<./Images/tap_setted.png>)

> Cuando se hace una conexión TCP correcta, se manda un código `200 OK` para confirmar que se ha establecido conexión. \
> Se debe buscar un paquete que descargue un archivo.

![packet_attached_file](<./Images/downloaded_file_packet.png>)

*Pregunta 2: What is the flag found in the DNS traffic in scenario 2? The flag has the format THM{}.* \
**Respuesta: THM{C2CommandFound}**

> Posicionar el TAP en 
el servidor DNS y capturar todo el tráfico.

![dns_tap](<./Images/dns_tap.png>)

> Se está buscando un archivo `.txt` ya que a través del archivo, se está inyectando un C2. \
> **RECORDATORIO**: El protocolo DNS viaja en el puerto 53.

```json
// Revisar con detenimiento las descripciones de los paquetes
Standard query response 0x41eb TXT c2.tryhackrne.thn
```

# Lecciones aprendidas
- Gracias al análisis de tráfico de red se pueden descubrir ataques como **DNS tunneling**, **ARP Poisoning** o **Fragmentation attack**.
- Se puede monitorear tanto el desempeño de la red como reconstruír un ataque a raíz de una respuesta a incidentes.
- Los firewalls no registran en su totalidad todo el contenido de los paquetes
- El modelo TCP/IP es para el análisis de tráfico de red.
- Los headers que aplica cada capa del modelo, son el punto de partida para el análisis de los paquetes.
- Hay que saber buscar a través de los logs de cada capa. Los logs tienen la información suficiente para descubrir el tipo de ataque que se está realizando.
- Los endpoints son los que generan prácticamente todo el tráfico de una red.
- Los logs son la primera línea de defensa pero existen otros métodos para visualizar el tráfico de red.
- Dependiendo de qué herramienta se va a usar (TAP o Port mirroring), se debe de posicionar en el lugar correcto o configurar correctamente el puerto.
- Se requiere gran cantidad de almacenamiento para el almacenamiento de los paquetes.
- Hay algunas herramientas especializadas para capturar los paquetes: Wireshark, TCPdump, IPS/IDS (snort), Suricata y Zeek

# References
[TryHackMe — Modelo OSI](<../../../Cybersecurity%20101/Network%20Concepts/TryHackMe — Modelo OSI.md>)