# TryHackMe — What is Networking
**Dificultad** -> easy | **Date** -> 16-jul-26 | **Type** -> Free \
**Sala** -> [What is networking?](https://www.tryhackme.com/room/whatisnetworking)

# Introducción
Sala enfocada en la introducción de conceptos básicos de las redes como la dirección IP y MAC.

# Solución
### Task 1 — What is networking?
Las redes pueden estar formadas desde 2 dispositivos hasta millones de ellos. Puede ser desde una laptop y un celular hasta cámaras de seguridad, semáforos e incluso en el sector agrario.

> Las redes son cosas conectadas

___
*Pregunta 1: What is the key term for devices that are connected together?* \
**Respuesta: Network**

### Task 2 — What is the internet?

> [!NOTE]
> **¿Qué es el Internet?** \
> El Internet es una red gigantesca formada por redes más pequeñas dentro de ella.

La primera versión del Internet fue **ARPANET** creada en la década de 1960 por el Departamento de Defensa de Estados Unidos. \
El Internet como se conoce actualmente fue creado en 1989 por **Tim Berners-Lee** con la creación del **WWW** (World Wide Web). El Internet se compone de dos tipos de redes:
* Redes privadas: Redes pequeñas aisladas.
* Redes públicas: Redes que conectan a las redes más pequeñas.

___
*Pregunta 1: Who invented the World Wide Web?* \
**Respuesta: Tim Berners-Lee**

### Task 3 — Identifying devices on a Network
En una red los dispositivos deben de identificarse y ser identificados para mantener el orden. Los dispositivos tienen dos formas de ser identificados en una red
1. Dirección IP: puede cambiar
2. Dirección MAC: no puede cambiar

> [!TIP]
> **Una analogía para ambas direcciones**
> - **Dirección IP**: similar al nombre propio de las personas. Se puede cambiar de forma legal.
> - **Dirección MAC**: similar a las huellas digitales de las personas. Son únicas para cada una y no se pueden cambiar.


| Dirección     | ¿Qué es?                                                                                                                                                                                                                                | Ejemplo                                                        |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Dirección IP  | Es un conjunto de números divididos por puntos (`.`) en 4 secciones llamadas octetos. Se dividen en dos grupos: IP pública e IP privada. Tienen una serie de protocolos que forzan a los dispositivos a comunicarse de la misma manera. | - IP privada: `192.168.200.30`<br>- IP pública: `86.157.52.21` |
| Dirección MAC | Es una interfaz física que se encuentra en la placa madre que se le asigna un valor único en la fábrica donde se construyó. Contiene 12 caracteres separados por dos puntos (`:`).                                                      | `A4:C3:FO:85:AC:2D`                                            |

> [!NOTE]
> **Acerca de la dirección MAC**  \
> La dirección MAC está compuesta por un número hexadecimal de 12 caracteres.

> [!IMPORTANT]
> **Anatomía de una MAC**
> - Los primeros seis dígitos de una dirección corresponden al fabricante de la interfaz
> - Los seis restantes corresponden a la dirección única de la interfaz

> Debido a la escasez de IPv4 se ha desarrollado el protocolo IPv6.

> [!WARNING]
> **Las MAC no son lo que aparentan** \
> El *Spoofing* consiste en un dispositivo fingiendo ser otro dentro de la misma red.

___
*Pregunta 1: What does the term "IP" stand for?* \
**Respuesta: Internet Protocol**

*Pregunta 2: What is each section of an IP address called?* \
**Respuesta: Octet**

*Pregunta 3: How many sections (in digits) does an IPv4 address have?* \
**Respuesta: 4**

*Pregunta 4: What does the term "MAC" stand for?* \
**Respuesta: Media Access Control**

*Pregunta 5: Deploy the interactive lab using the "View Site" button and spoof your MAC address to access the site.  What is the flag?* \
**Respuesta: THM{YOU_GOT_ON_TRYHACKME}**

> **NOTA**: Hacer spoofing

### Task 4 — Ping (ICMP)
Ping es una de las herramientas más fundamentales para comprobar una red. Utiliza paquetes **ICMP** (Internet Control Message Protocol) para determinar el desempeño de red entre dos dispositivos.

> [!NOTE]
> Ping mide el tiempo que tardan los paquetes en viajar entre un dispositivo a otro mediante el echo del paquete **ICMP** y posteriormente la respuesta de echo del receptor.

___
*Pregunta 1: What protocol does ping use?* \
**Respuesta: ICMP**

*Pregunta 2: What is the syntax to ping 10.10.10.10?* \
**Respuesta: ping 10.10.10.10**

*Pregunta 3: What flag do you get when you ping 8.8.8.8?* \
**Respuesta: THM{I_PINGED_THE_SERVER}**

# Lecciones aprendidas
- En términos sencillos, el Internet es una red de redes.
- El "abuelo" de Internet fue la **ARPANET** en la década de 1960.
- El Internet moderno surgió en 1989 gracias a **Tim Berners-Lee**
- La dirección MAC no puede cambiar, está grabada en la interfaz de red desde la fábrica que lo hizo
	- Las direcciones MAC pueden ser suplantadas.
	- Las direcciones MAC se dividen en dos partes.
- Las IP se dividen en IP's públicas y privadas.
- Las direcciones IPv4 se están acabando, por tal motivo se ha creado el protocolo IPv6 que son 2^32 direcciones IP públicas.
- Ping utiliza el protocolo **ICMP** para comprobar la conexión entre dos dispositivos.