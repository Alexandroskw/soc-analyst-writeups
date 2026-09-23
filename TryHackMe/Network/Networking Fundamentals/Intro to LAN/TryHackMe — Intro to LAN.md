# TryHackMe — Intro to LAN
**Dificultad** -> easy | **Date** -> 16-jul-26 | **Type** -> Free \
**Weblink** -> [Intro to LAN](https://www.tryhackme.com/room/introtolan)

# Introduction
Sala enfocada en la introducción a una red de área local (LAN), sus diferentes topologías y el subnetting.

# Solución
### Task 1 — Introducing LAN topologies
El término *topología* en el contexto de las redes se refiere al diseño o la estructura de una red

| Topología | Descripción                                                                                       | Pros y contras                                                                                                                                                             |
| :-------: | ------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|   Star    | Un solo nodo central que puede ser un Switch o un Hub maneja toda la red.                         | - Sencilla de implementar y mantener<br>- Se debe comprar nuevo equipo cada que se aumentar su tamaño                                                                      |
|    Bus    | Único cable (bus) central donde se conectan todos los dispositivos similar al tronco de un árbol. | - Si el bus se daña, toda la infraestructura se queda sin conexión<br>- Propensa a tener cuellos de botella                                                                |
|   Ring    | Los dispositivos se conectan secuencialmente formando un bucle. (Similar a la anterior)           | - Los nodos solo pueden enviar datos de otro dispositivo si no tienen nada mas que enviar<br>- Es unidireccional<br>- Siempre enviará sus datos antes que los de otro nodo |

#### Dispositivos de red

| Dispositivo | ¿Qué hace?                                                                                                                                                                                                         |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Switch      | Están diseñados para agrupar cualquier otro dispositivo capaz de conectarse a la red mediante Ethernet. Cuentan con 4, 8, 16, 24, 32 o 64 puertos, lo cual permite agregar más dispositivos (incluyendo switches). |
| Róuter      | Permite conectar otras redes y pasar datos entre ellas mediante el enrutamiento, de ahí su nombre.                                                                                                                 |

___
*Pregunta 1: What does LAN stand for?* \
**Pregunta: Local Area Network**

*Pregunta 2: What is the verb given to the job that Routers perform?* \
**Respuesta: Routing**

*Pregunta 3: What device is used to centrally connect multiple devices on the local network and transmit data to the correct location?* \
**Respuesta: Switch**

*Pregunta 4: What topology is cost-efficient to set up?* \
**Respuesta: Bus topology**owner:Alexandroskw 

*Pregunta 5: What topology is expensive to set up and maintain?* \
**Respuesta: Star topology**

*Pregunta 6: Complete the interactive lab attached to this task. What is the flag given at the end?* \
**Answer: THM{TOPOLOGY_FLAWS}**

![](<Pasted image 20260717013933.png>)
![](<Pasted image 20260717014011.png>)
![](<Pasted image 20260717014108.png>)
![](<Pasted image 20260717014139.png>)
![](<Pasted image 20260717014252.png>)
![](<Pasted image 20260717014323.png>)
![](<Pasted image 20260717014343.png>)
![](<Pasted image 20260717014359.png>)
![](<Pasted image 20260717014424.png>)
![](<Pasted image 20260717014443.png>)

### Task 2 — A primer on subnetting
The subnetting is splitting up a network in more small networks itself.
![](<Pasted image 20260717205558.png>)
A subnet mask is represented in a 32-bit (4 bytes) number 0 - 255
Networks needs to know where to send the data like the network administrator. The network admin uses the subnettig to categorise and assign parts of the network.
The subnetting uses the IP address in three different ways

| Type            | Purpose                                                   | Explanation                                                                                                                                                                                     | Example        |
| --------------- | --------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| Network address | Identifies the start of the actual network                | A device can have the IP `192.168.1.20` will be in the net `192.168.1.0`                                                                                                                        | 192.168.1.0    |
| Host address    | The IP of the device in the net                           | Your phone can have the IP `192.168.1.55`                                                                                                                                                       | `192.168.1.55` |
| Default gateway | Is a IP capable of sending information to another network | If you wanna send information to the sales departament in a different network the data will be sent to this IP. usually use either the first or last host address in a network (`.1` or `.254`) | 192.168.11.1   |

___
*Pregunta 1: What is the technical term for dividing a network up into smaller pieces?* \
**Answer: Subnettig**

*Pregunta 2: How many **bits** are in a subnet mask?* \
**Answer: 32**

> Using the hint in this. I don't remember so well

*Pregunta 3: What is the range of a section (octet) of a subnet mask?* \
**Answer: 0-255**

Remember the basics of the IP Address in the last room

*Pregunta 4: What address is used to identify the start of a network?* \
**Answer: Network address**

*Pregunta 5: What address is used to identify devices within a network?* \
**Answer: Host address**

*Pregunta 6: What is the name used to identify the device responsible for sending data to another network?* \
**Answer: Default gateway**

## Task 3 — ARP
Address Resolution Protocol is the intermediary between IP Address and MAC address. Its the responsable to find the iterface (MAC) associated to an IP that is being requested.
When a device wants communicate with another device, it send a broadcast (ARP request) to the entire network asking for the IP address that is being requested and awaits for a answer of the device has it (ARP reply).

![](<Pasted image 20260717223503.png>)

___
*Pregunta 1: What does ARP stand for?*
**Answer: Address Resolution Protocol**

*Pregunta 2: What category of ARP Packet asks a device whether or not it has a specific IP address?*
**Answer: Request**

*Pregunta 3: What address is used as a physical identifier for a device on a network*
**Answer: MAC Address**

*Pregunta 4: What address is used as a logical identifier for a device on a network?*
**Answer: IP Address**

## DHCP
IP can be assigned manually by the user in the device or in most common cases would be automatic with the DHCP (Dynamic Host Configuration Protocol). When a device connects to a network and if the user not assigned an IP manually, it sends a **DHCP Discover** package it any DHCP server is on the network. Then the DHCP server replies back with an **DHCP Offer**, to offer a new IP address. The device will confirm it wants the IP with **DHCP Request** and lastly the DHCP server will reply aknowledging this process witt **DHCP ACK**

![](<Pasted image 20260717225243.png>)

___
*Pregunta 1: What type of DHCP packet is used by a device to _retrieve an IP address?_* \
**Answer: DHCP Discover**

*Pregunta 2: What type of DHCP packet does a device **send once it has been** **offered an IP address** by the DHCP server?* \
**Answer: DHCP Request**

*Pregunta 3: Finally, **what is the last** DHCP packet that is sent to a device from a DHCP server?* \
**Answer: DHCP ACK**
