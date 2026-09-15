**Difficult** -> easy
**Date** -> 16-jul-26
**Type** -> Training
**Weblink** -> [https://www.tryhackme.com/room/introtolan](Intro to LAN)
## Introduction
Se da una breve introducción de qué es una LAN (Local Area Network), que son las redes privadas.
## Solution
### Task 1 — Introducing LAN topologies
#### Star topology (Topología en estrella)
Un solo nodo central que puede ser un Switch o un Hub maneja toda la red.
![[Pasted image 20260717005518.png]]
Debido a su propia naturaleza, es la más sencilla de implementar y mantener a pesar del costo elevado ya que se debe de comprar nuevo equipo cada que se quiera aumentar el tamaño de la red.
Si la red crece, los problemas también, ya que se se hace muy difícil mantenerla. Si el dispositivo central falla, ya no se envían ni reciben nuevos datos.
#### Bus topology (topología de bus)
Es una topología bastante sencilla, consiste en un único cable (bus) central donde se conectan todos los dispositivos similar al trono de un árbol. Sin embargo, si algo malo le ocurre a este cable central, se pierde toda la conexión en los dispositivos.
![[Pasted image 20260717010600.png]]
Otra desventaja es que es mas propensa a tener cuellos de botella si todos los dispositivos están pidiendo datos y se vuelve complicado identificar qué dispositivo es el que lo está ocasionando ya que los datos pedidos de todos los dispositivos van en el mismo tronco común.
#### Ringo topology (topología en anillo)
Similar a la topología anterior en el concepto de un único bus. Los dispositivos se conectan secuencialmente formando un bucle (de ahí el nombre de anillo). Los datos se envían a través del bucle utilizando los demás nodos hasta que llegan al dispositivo objetivo.
![[Pasted image 20260717011150.png]]
Los nodos solo pueden enviar datos de otro dispositivo si no tienen nada que enviar. Es unidireccional, es decir, los datos viajarán en un solo sentido. Siempre enviará sus propios datos antes que los de otro nodo. Debido a su naturaleza, es muy fácil resolver sus problemas si es que surjen.
#### ¿Qué es un switch?
Son dispositivos que se agregan a una red para expandirla agregando otros dispositivos a sus puertos. Anteriormente se utilizaban hubs que repetían el paquete en todos los puertos. El switch, almacena en qué puerto está conectado cada dispositivo y envía el paquete correspondiente evitando colisiones de paquetes y por lo tanto, pérdidas de datos
![[Pasted image 20260717012936.png]]
#### ¿Qué es un router?
Como su nombre lo indica, se encarga de enrutar el tráfico de una red a otra o dentro de la misma red. El enrutamiento es el nombre que se le da al viaje de los datos a través de una red. El router se encarga de encontrar la ruta más corta de un dispositivo a otro para que los datos sean entregados satisfactoriamente. El enrutamiento funciona mejor cuando hay mas de un router involucrado.
![[Pasted image 20260717013347.png]]
*Question 1: What does LAN stand for?*
**Answer: Local Area Network**
*Question 2: What is the verb given to the job that Routers perform?*
**Answer: Routing**
*Question 3: What device is used to centrally connect multiple devices on the local network and transmit data to the correct location?*
**Answer: Switch**
*Question 4: What topology is cost-efficient to set up?*
**Answer: Bus topology**
*Question 5: What topology is expensive to set up and maintain?*
**Answer: Star topology**
*Question 6: Complete the interactive lab attached to this task. What is the flag given at the end?*
![[Pasted image 20260717013933.png]]
![[Pasted image 20260717014011.png]]
![[Pasted image 20260717014108.png]]
![[Pasted image 20260717014139.png]]
![[Pasted image 20260717014252.png]]
![[Pasted image 20260717014323.png]]
![[Pasted image 20260717014343.png]]
![[Pasted image 20260717014359.png]]
![[Pasted image 20260717014424.png]]
![[Pasted image 20260717014443.png]]

**Answer: THM{TOPOLOGY_FLAWS}**
### Task 2 — A primer on subnetting
The subnetting is splitting up a network in more small networks itself.
![[Pasted image 20260717205558.png]]
A subnet mask is represented in a 32-bit (4 bytes) number 0 - 255
Networks needs to know where to send the data like the network administrator. The network admin uses the subnettig to categorise and assign parts of the network.
The subnetting uses the IP address in three different ways

| Type            | Purpose                                                   | Explanation                                                                                                                                                                                     | Example        |
| --------------- | --------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| Network address | Identifies the start of the actual network                | A device can have the IP `192.168.1.20` will be in the net `192.168.1.0`                                                                                                                        | 192.168.1.0    |
| Host address    | The IP of the device in the net                           | Your phone can have the IP `192.168.1.55`                                                                                                                                                       | `192.168.1.55` |
| Default gateway | Is a IP capable of sending information to another network | If you wanna send information to the sales departament in a different network the data will be sent to this IP. usually use either the first or last host address in a network (`.1` or `.254`) | 192.168.11.1   |
>*Question 1: What is the technical term for dividing a network up into smaller pieces?*
 **Answer: Subnettig**

>*Question 2: How many **bits** are in a subnet mask?*
 Using the hint in this. I don't remember so well
 **Answer: 32**

>*Question 3: What is the range of a section (octet) of a subnet mask?*
 Remember the basics of the IP Address in the last room
 **Answer: 0-255**

> *Question 4: What address is used to identify the start of a network?*
> **Answer: Network address**

> *Question 5: What address is used to identify devices within a network?*
> **Answer: Host address**

> *Question 6: What is the name used to identify the device responsible for sending data to another network?*
> **Answer: Default gateway**

## Task 3 — ARP
Address Resolution Protocol is the intermediary between IP Address and MAC address. Its the responsable to find the iterface (MAC) associated to an IP that is being requested.
When a device wants communicate with another device, it send a broadcast (ARP request) to the entire network asking for the IP address that is being requested and awaits for a answer of the device has it (ARP reply).
![[Pasted image 20260717223503.png]]
> *Question 1: What does ARP stand for?*
> **Answer: Address Resolution Protocol**

> *Question 2: What category of ARP Packet asks a device whether or not it has a specific IP address?*
> **Answer: Request**

> *Question 3: What address is used as a physical identifier for a device on a network*
> **Answer: MAC Address**

> *Question 4: What address is used as a logical identifier for a device on a network?*
> **Answer: IP Address**

## DHCP
IP can be assigned manually by the user in the device or in most common cases would be automatic with the DHCP (Dynamic Host Configuration Protocol). When a device connects to a network and if the user not assigned an IP manually, it sends a **DHCP Discover** package it any DHCP server is on the network. Then the DHCP server replies back with an **DHCP Offer**, to offer a new IP address. The device will confirm it wants the IP with **DHCP Request** and lastly the DHCP server will reply aknowledging this process witt **DHCP ACK**
![[Pasted image 20260717225243.png]]
> *Question 1: What type of DHCP packet is used by a device to **retrieve an IP address?**
> **Answer: DHCP Discover**

> *Question 2: What type of DHCP packet does a device **send once it has been** **offered an IP address** by the DHCP server?*
> **Answer: DHCP Request**

> *Question 3: Finally, **what is the last** DHCP packet that is sent to a device from a DHCP server?*
> **Answer: DHCP ACK**