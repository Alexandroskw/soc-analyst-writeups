# TryHackMe — Network Traffic Basics
**Dificultad** -> easy | **Date** -> 17-nov-26 | **Type** -> Free \
**Sala** -> [Network Traffic Basics](https://www.tryhackme.com/room/networktrafficbasics)
## Introducción
Sala enfocada en el análisis del flujo de una red. Desde la captura, inspección y el análisis de los paquetes de red, no solo enfocado a la herramienta **Wireshark**.

## Solución
### DNS tunneling y beaconing
Basados en los logs de DNS se puede obtener la siguiente información
- Consulta y tipo de consulta (query y querytype)
- Subdominio y top-level domain. (Se puede consultar AbuseIPDB y VirusTotal para conocer la naturaleza del dominio)
- Host IP identificado enviando las solicitudes de DNS
- IP de destino (se puede utilizar AbuseIPDB o VirusTotal para verificar si la IP está marcada como sospechosa)
- Timestamp para construir una línea de tiempo de las solicitudes maliciosas

> [!IMPORTANT]
> Los logs DNS solo dan esta información. Si se requiere obtener más información, se debe inspeccionar más en detalle el tráfico DNS para revisar las peticiones y respuestas.

> [!TIP]
> Los firewalls y otros dispositivos registran las peticiones y respuestas DNS pero no su contenido.

| NTA básico                                                                   | Perspectiva de un SOC                                  |
| ---------------------------------------------------------------------------- | ------------------------------------------------------ |
| Monitoreo de desempeño                                                       | Detectar actividad maliciosa o sospechosa              |
| Buscar anormalidades en la red                                               | Reconstruir ataques durante una respuesta a incidentes |
| Inspeccionar el contenido de comunicaciones sospechosas, internas y externas | Verificar y validar alertas                            |

A través del tráfico DNS se puede filtrar información, esta técnica se conoce como **DNS Tunneling**.

### Modelo TCP/IP
En cada capa del modelo TCP/IP, se requiere cierta información para pasar los datos a la siguiente capa, a esta información se le conoce como **header**. Cada información incluida en cada header junto con los datos de aplicación (application data) es lo que se requiere observar.

> Los logs usualmente incluyen bits y partes del header pero nunca los detalles completos del paquete.

#### Capa de Aplicación (Application layer)
En esta capa existen dos tipos de estructuras, la información del header de aplicación y los datos de la aplicación en sí (La información puede variar con base en qué protocolo de la capa de aplicación se esté utilizando).

> [!NOTE]
> Algunos proxies y firewalls registran estos datos del header pero no registran son los datos de aplicación o el payload.

Cuando un cliente hace una solicitud `GET` al servidor, (el servidor) responde con un código 200 (`200 OK`), es decir, la solicitud ha sido aceptada. De la solicitud `GET` se puede determinar que el cliente puede estar solicitando un archivo para descargar sin embargo, en el registro no se puede ver el contenido del archivo solicitado.

#### Capa de Transporte (Transport layer)
Los datos de aplicación (y su header) se parten en pequeños pedazos y se encapsulan en la capa de transporte, en cada fragmento se incluye el header de esta capa (usualmente `TCP` o `UDP`).

> En los logs de firewall usualmente se incluyen los puertos de origen y destino, y las banderas pero otros campos no son incluidos.

Los campos que se muestra en un log de firewall son valiosos para detectar cierto tipo de ataques como el **Session Hijacking**. Para detectarla, se puede analizar la secuencia de números incluidos en el header. Si la secuencia da un salto muy brusco en la secuencia se necesita hacer una investigación.

#### Capa de Internet (Internet layer)
Cuando la capa de transporte le manda el segmento, la capa de Internet también le agregará su propio header. Si el segmento es más grande que la Maximum Transmission Unit (**MTU**), lo fragmentará y le agregará el header a cada fragmento (los campos que usualmente se registran son la IP de origen y destino, y el TTL).

Para detectar los ataques por fragmentación (**fragmentation attack**), se necesitan otros campos, como el *total length* y el *fragment offset*. Un atacante puede, por ejemplo, mandar pequeños fragmentos para evadir el IDS o corromper el reensamblaje de los fragmentos utilizando rangos de bytes superpuestos.

#### Capa de Enlace (Link layer)
Cuando se termina el encapsulamiento en la capa anterior, el paquete IP pasa a la capa de enlace que agrega su propio header contendiendo más información de direccionamiento (algunos registros muestran la dirección MAC de origen y destino). \
Para ciertos tipos de ataque como el envenenamiento ARP (ARP poisoning) o el spoofing, esta información no es suficiente. Para estos tipos de ataque se requiere todo el contexto del paquete.

### Network traffic sources and flows
#### Sources

| Tipo de fuente | Enfoque                                                                                                | ¿Quiénes son?                                                                                            |
| -------------- | ------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------- |
| Intermediarios | Dispositivos por donde pasa la mayoría del tráfico de red                                              | Firewalls, switchs, proxies, IDS/IPS, róuters, access points, Wi-Fi, entre otros.                        |
| Endpoints      | Dispositivos que generan la mayor parte del tráfico. Consumen la mayoría del ancho de banda de la red. | Servidores, hosts, dispositivos IoT, impresoras, recursos de la nube, dispositivos móviles, entre otros. |

#### Flows

| Tipos       | ¿Dónde están?                                                            | ¿Quién los genera?                                                                                                                                                                                                           |
| ----------- | ------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| North-South | Tráfico que sale o entra en la LAN y pasa el firewall                    | HTTPS, DNS, SSH, SMTP, VPN, RDP (entre otros)                                                                                                                                                                                |
| East-West   | Tráfico que se queda en la LAN. Incluye la LAN que se extiende a la nube | - Directory, Authentication & Identity Services <br>- File shares & print services<br>- Router, switching, and infrastructure services<br>- Application Communication<br>- Backup & Replication<br>- Monitoring & Management |

#### Flow examples
|       Flujo       | ¿Qué hace?                                                                                                                                                                                                                                                                              |
| :---------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|       HTTPS       | Se tiene dos sesiones: una entre el cliente y el proxy, y otra entre el proxy y el servidor web. Desde el punto de vista del cliente, este ha establecido una sesión con el servidor web.                                                                                               |
|   External DNS    | El host envía la consulta al servidor DNS interno en el puerto 53, el cual actuará en nombre del host. Verificará si tiene una respuesta a la consulta en su caché. De no ser así enviará la consulta a través del enrutador pasando por el firewall a los servidores DNS configurados. |
| SMB with Kerberos | Al abrir un recurso compartido se crea una sesión SMB.<br>El equipo usa Kerberos: con el Ticket Granting Ticket (TGT) ya obtenido, pide un ticket de servicio y lo usa para conectarse y acceder al recurso.                                                                            |
### Task 5 — How we can observe network traffic?
Se puede obtener información para el análisis de tráfico de red de varias formas: Logs, full packet capture y network statistics.

| Fuente              | ¿Qué sucede                                                                                                                                                                                                                                                                                                                          |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Logs                | Es la primer línea de defensa de lo que ocurre en la red. Los datos que se registran dependen del proveedor.                                                                                                                                                                                                                         |
| Full packet capture | - **TAP de red físico**: dispositivo físico que se pone en la red para capturar todo el tráfico y posteriormente se envía a un IDS o algún otro sistema dedicado.<br>- **Port mirroring**: es un software que copia los paquetes de un dispositivo y lo envía a otro dentro de la misma red para monitorearlos, por ejemplo, un IDS. |
| Network statistics  | - **NetFlow**: es un protocolo desarrollado por Cisco que recopila metadatos sobre el tráfico que circula por una red.<br>- **IPFIX**: En colaboración con Cisco y otros proveedores, el IETF creó IPFIX y lo publicó como un estándar independiente de proveedores.:LiAArrowDown:                                                   |

> [!NOTE]
> **Acerca del TAP**
> El TAP opera en la capa de enlace. No necesita la dirección MAC o IP porque copia las señales eléctricas y las envía al puerto de monitoreo.

#### Best practices
Cuando se vaya a hacer una captura de paquetes, es necesario tener en cuenta

Posición: dependiendo de qué tráfico queremos capturar, necesitamos posicionar correctamente el TAP o configurar el mirror correctamente.
Duración: la captura completa de paquetes necesita una cantidad de almacenamiento considerable. Si se captura 1Gbps de un día entero, posiblemente necesitemos 10.8 TB de almacenamiento.
Mirror vs. TAP: los TAP's ofrecen una reducción del desempeño casi nula. El mirroring puede agregar latencia cuando una gran cantidad de tráfico está ocurriendo a través del puerto de mirroring.
## Lecciones aprendidas
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

#### Referencias
[TryHackMe — Network Traffic Basics](TryHackMe/Network/Network%20Traffic%20Basics/TryHackMe%20—%20Network%20Traffic%20Basics.md)