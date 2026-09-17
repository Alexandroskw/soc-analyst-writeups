# TryHackMe — Modelo OSI
**Dificultad** -> easy | **Date** -> 04-sep-26 | **Type** -> Free
**Sala** -> [Networking Concepts](https://tryhackme.com/room/networkingconcepts)
___
## Introducción
Sala enfocada en los modelos OSI y TCP/IP de Internet y las diferencias entre ambos.
## Solución
### Task 2 — OSI Model
Es un modelo desarrollado por el **ISO** (International Organization for Standarization). Es un framework que describe cómo es que las comunicaciones deberían de ocurrir entre las redes informáticas. Por lo tanto es más un modelo teórico que un estádar en si.

El modelo OSI se compone de 7 capas:
1. **Física** (physic)
2. **Enlace de datos** (data link)
3. **Red** (Network)
4. **Transporte** (Transport)
5. **Sesión** (Session)
6. **Presentación** (Presentation)
7. **Aplicación** (Application)

> [!TIP] Mnemotecnia, un gran aliado
> Se pueden utilizar mnemotecnias para recordar el orden de las capas como:
> **Please Do Not Throw Spinach Pizza Away**
> **All People Seem To Need Data Processing**

| Capa   | Nombre       | Descripción                                                       | Protocolos                                 |
| ------ | ------------ | ----------------------------------------------------------------- | ------------------------------------------ |
| Capa 7 | Application  | Proporciona los servicios e interfaces para las aplicaciones.     | HTTP, FTP, DNS, POP3, SMTP, IMAP           |
| Capa 6 | Presentation | Cifrado, descifrado y compresión de datos,                        | Unicode, ASCII, JPEG, PNG                  |
| Capa 5 | Session      | Establece, mantiene y sincroniza la sesión entre dispositivos.    | NFS, RPC                                   |
| Capa 4 | Transport    | Comunicación de extremo a extremo y segmentación de los datos.    | TCP, UDP                                   |
| Capa 3 | Network      | Direccionamiento lógico y enlace con otras redes.                 | IP, ICMP, IPSec                            |
| Capa 2 | Data Link    | Transferencia de datos confiable entre los nodos de una misma red | Ethernet (802.3), Wi-Fi (802.11)           |
| Capa 1 | Physical     | Transimsión física de los datos                                   | Señales eléctricas, ópticas o inalámbricas |
___
*Pregunta 1: Which layer is responsible for end-to-end communication between running applications?*
**Respuesta: 2**

*Pregunta 2: Which layer is responsible for routing packets to the proper network?*
**Respuesta: 3**

*Pregunta 3: In the OSI model, which layer is responsible for encoding the application data?*
**Respuesta: 6**

*Pregunta 4: Which layer is responsible for transferring data between hosts on the same network segment?*
**Respuesta: 2**
### Task 3 — TCP/IP Model
**TCP/IP** son las siglas de *Transmission Control Protocol/Internet Protocol* fue desarrollado en la década de 1970 por el Departamento de Defensa de los Estados Unidos. Una de las fortalezas de este modelo es que puede funcionar aún cuando algunas de sus partes están fuera de servicio (como en un ataque militar).

| Número de capa | Modelo OSI   | Modelo TCP/IP | Protocolos                                      |
| -------------- | ------------ | ------------- | ----------------------------------------------- |
| 7              | Application  | Application   | HTTP, HTTPS, FTP, POP3, SMTP, IMAP, Telnet, SSH |
| 6              | Presentation | Application   |                                                 |
| 5              | Session      | Application   |                                                 |
| 4              | Transport    | Transport     | TCP, UDP                                        |
| 3              | Network      | Internet      | IP, ICMP, IPSec                                 |
| 2              | Data link    | Link          | Ethernet (802.3), Wi-Fi (802.11)                |
| 1              | Physical     | Link          |                                                 |
> [!WARNING]
> Algunos libros de texto ponen al Modelo **TCP/IP** con 5 capas en lugar de 4.
> Separando la Capa física de la Capa de Enlace.

___
*Pregunta 1: To which layer does HTTP belong in the TCP/IP model?*
**Respuesta: Application Layer**

*Pregunta 2: How many layers of the OSI model does the application layer in the TCP/IP model cover?*
**Respuesta: 3**
### Task 4 — IP adresses and subnets
Todos los dispositivos en una red necesitan un identificador único para que los demás dispositivos se comuniquen con él. Las direcciones IP se componen de 4 octetos que van de 0 a 255.

> Dirección **IPv4**: 192.168.1.0


> [!INFO]
> Las direcciones 0 y 255 están reservadas para la dirección de red y la de broadcas respectivamente.

Las direcciones IP se dividen en 2 tipos: **IP privada** e **IP pública**.
La IP privada no puede alcanzar el internet por sí sola, necesita un róuter para que contenga una IP pública y un protocolo NAT (Network Address Translator).

Rangos de direcciones privadas más comúnes:

|      Rango de direcciones       | Máscara CIDR | Tipo |
|:-------------------------------:|:------------:|:----:|
|   `10.0.0.0 - 10.255.255.255`   |     `/8`     |  A   |
|  `172.16.0.0 - 172.32.255.255`  |    `/12`     |  B   |
| `192.168.0.0 - 192.168.255.255` |    `/16`     |  C   |

___
*Pregunta 1: Which of the following IP addresses is not a private IP address?*
**Respuesta: `49.69.147.197`**

> Recordar las direcciones IP privadas más comúnes

*Pregunta 2: Which of the following IP addresses is not a valid IP address?*
**Respuesta: `192.168.305.19`**

> Recordar qué es un octeto
### Task 5 — TCP & UDP
El protocolo **UDP** (User Datagram Protocol) es un protocolo sin conexión que opera en la capa de transporte.

> [!INFO]
> _Protocolo sin conexión_ hace referencia a que no necesita establecer una conexión para empezar a transportar los paquetes ni se asegura que los
> paquetes llegan al destino. 

El protocolo **TCP** (Transmission Control Protocol) es un protocolo orientado a conexión. Al igual que el Protocolo UDP, se encuentra en la capa de transporte.

> [!INFO]
> _Protocolo orientado a conexión_ hace referencia a que necesita establecer una conexión para empezar a transportar los paquetes y por lo tanto, se asegura
> que los paquetes llegan a su destino.

> A la conexión del protocolo TCP se le conoce como **Three-Way Handshake**.

|        Fases        | Descripción                                                                                                       |
|:-------------------:| ----------------------------------------------------------------------------------------------------------------- |
|   Paquete **SYN**   | El cliente pide iniciar la conexión enviando el paquete al servidor.                                              |
| Paquete **SYN-ACK** | El servidor responde enviando de vuelta al cliente el paquete.                                                    |
|   Paquete **ACK**   | La conexión se establece cuando el cliente envía el paquete, dando a conocer que ha recibido el paquete anterior. |
___
*Pregunta 1: Which protocol requires a three-way handshake?*
**Respuesta: TCP**

*Pregunta 2: What is the approximate number of port numbers (in thousands)?*
**Respuesta: 65**

> Recordar que los puertos van de 1 a 65535
### Task 6 — Encapsulation
En términos simples, la encapsulación se refiere a que cada capa agraga su propia cabecera (**header**) para posteriormente enviarla a la capa inferior ya encapsulados los datos.

| Fase                       | ¿Qué pasa?                                                                                                                   |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Application data           | El usuario hace una petición. La aplicación hace da formato a los datos con base al protocolo que se le definió (TCP o UDP). |
| Transport protocol segment | Agrega el header adecuado y crea un segmento TCP o un datagrama UDP según sea el caso.                                       |
| Network packet             | Agrega el header IP al segmento o datagrama recibido                                                                         |
| Data link Frame            | El ethernet o Wi-Fi reciben el paquete IP y agregan el header y trailer. Se crea el *frame*.                                 |

> [!NOTE]
> Para que el receptor pueda utilizar los datos, el proceso es a la inversa y se le conoce como *Desencapsulamiento*.

___
*Pregunta 1: On a WiFi, within what will an IP packet be encapsulated?*
**Respuesta: Frame**

> En la última etapa se crean los frames

*Pregunta 2: What do you call the UDP data unit that encapsulates the application data?*
**Respuesta: Datagram**

*Pregunta 3: What do you call the data unit that encapsulates the application data sent over TCP?*
**Respuesta: Segment**
### Task 7 — Telnet
El protocolo `telnet` (Teletype Network) permite la conexión remota a una terminal (similar al `SSH`).

> [!CAUTION]
> Los protocolos `echo` y `daytime` en los puertos 7 y 13 (TCP y UDP)
> respectivamente, son considerados riesgos de seguridad.

#### Conectando al laboratorio

> Conectarse al laboratorio mediante AttackBox

```bash
# Conectando al servidor telnet (La "IP_MACHINE" varía)
telnet <IP_MACHINE> 80

# Hacer una petición GET al servidor y especificar el header del host
GET / HTTP/1.1
Host: telnet.thm

# Presionar dos veces Enter y aparecerá la respuesta
HTTP/1.1 200 OK
Content-Type: text/html
ETag: "2920831920"
Last-Modified: Thu, 20 Jun 2024 12:39:38 GMT
Content-Length: 20
Accept-Ranges: bytes
Date: Mon, 07 Sep 2026 21:58:45 GMT
Server: lighttpd/1.4.63

THM{TELNET_MASTER}
```

___
*Pregunta 1: Use `telnet` to connect to the web server on `10.67.148.190`. What is the name and version of the HTTP server?*
**Respuesta: `lighttpd/1.4.63`**

*Pregunta 2: What flag did you get when you viewed the page?*
**Respuesta: THM{TELNET_MASTER}**
## Lecciones aprendidas
+ El Modelo OSI es más una referencia de cómo debería aplicarse la comunicación entre dispositivos que un modelo aplicable.
+ El Modelo TCP/IP es el estándar del Internet moderno.
+ Algunos autores van a separar el Modelo TCP/IP en 5 capas, otros los dejarán en 4.
+ La mnemotecnia es una gran forma de memorizar y recordar todas las capas del Modelo OSI.
+ Las direcciones IP privadas tienen 3 grandes grupos enfocados en diferentes aplicaciones.
+ El protocolo **UDP** no le interesa si el paquete llega a salvo al destino a diferencia del **TCP** que primero establece una conexión y se asegura que llegue el paquete al destino.
+ El **Three-Way Handshake** es la forma en la que TCP establece la conexión.
+ El encapsulamiento es la agregación de un header que identifica a cada capa durante el proceso.
+ La desencapsulación es el proceso a la inversa de la encapsulación.
+ `telnet` es utilizado para conectarse a una terminal remotamente, igual a `SSH`.