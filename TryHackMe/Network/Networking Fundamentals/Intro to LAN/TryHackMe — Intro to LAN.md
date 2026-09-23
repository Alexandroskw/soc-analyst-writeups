# TryHackMe — Intro to LAN
**Dificultad** -> easy | **Date** -> 16-jul-26 | **Type** -> Free \
**Sala** -> [Intro to LAN](https://www.tryhackme.com/room/introtolan)

# Introducción
Sala enfocada en la introducción a una red de área local (**LAN**), sus diferentes topologías y el subnetting.

# Solución
### Task 1 — Introducing LAN topologies

> El término *topología* en el contexto de las redes se refiere al diseño o la estructura de una red

| Topología | Descripción                                                                                       | Pros y contras                                                                                                                                                                                           |
| :-------: | ------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|   Star    | Un solo nodo central que puede ser un Switch o un Hub maneja toda la red.                         | - Sencilla de implementar y mantener<br>- Se debe comprar nuevo equipo cada que se aumentar su tamaño                                                                                                    |
|    Bus    | Único cable (bus) central donde se conectan todos los dispositivos similar al tronco de un árbol. | - Si el bus se daña, toda la infraestructura se queda sin conexión<br>- Propensa a tener cuellos de botella                                                                                              |
|   Ring    | Los dispositivos se conectan secuencialmente formando un bucle. (Similar a la anterior)           | - Los nodos solo pueden enviar datos de otro dispositivo si no tienen nada mas que enviar<br>- Es unidireccional<br>- Siempre enviará sus datos antes que los de otro nodo<br>- Fácil de detectar fallos |

#### Dispositivos de red

| Dispositivo | ¿Qué hace?                                                                                                                                                                                                   |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Switch      | Diseñados para agrupar cualquier otro dispositivo capaz de conectarse a la red mediante Ethernet. Cuentan con 4, 8, 16, 24, 32 o 64 puertos, lo cual permite agregar más dispositivos (incluyendo switches). |
| Róuter      | Permite conectar otras redes y pasar datos entre ellas mediante el enrutamiento, de ahí su nombre.                                                                                                           |

> [!TIP]
> **¿Qué es el _Routing_?** \
> Nombre que se le da al proceso por el cual los datos viajan a través de las redes. \
> El enrutamiento crea una ruta entre las redes para que los datos sean entregados exitosamente

___
*Pregunta 1: What does LAN stand for?* \
**Respuesta: Local Area Network**

*Pregunta 2: What is the verb given to the job that Routers perform?* \
**Respuesta: Routing**

*Pregunta 3: What device is used to centrally connect multiple devices on the local network and transmit data to the correct location?* \
**Respuesta: Switch**

*Pregunta 4: What topology is cost-efficient to set up?* \
**Respuesta: Bus topology**

> **RECORDATORIO**: Usa un único cable.

*Pregunta 5: What topology is expensive to set up and maintain?* \
**Respuesta: Star topology**

> **NOTA**: Los datos deben seguir un único flujo

*Pregunta 6: Complete the interactive lab attached to this task. What is the flag given at the end?* \
**Respuesta: THM{TOPOLOGY_FLAWS}**

### Task 2 — A primer on subnetting
El subnetting se refiere a la técnica para dividir una red grande en varias redes más pequeñas dentro de sí misma.

> Se utiliza para clasificar y asignar partes de una red a un tipo de información en específico.

> [!NOTE]
> Las IPv4 se dividen en 4 secciones llamadas octetos, al igual que las máscaras de red.

> [!IMPORTANT]
> Las IP y las máscaras de subred contienen 32-bits repartidos en los cuatro octetos formando 4 bytes. El rango de los octetos va de `0` a `255`.

|      Tipo       | Propósito                                                                                                                   | Explicación                                                                                                          |     Ejemplo     |
| :-------------: | --------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- | :-------------: |
| Network Address | Identifica el inicio de la actual red y es usada para identificar la existencia de la misma                                 | Un dispositivo con una IP `192.168.1.100` será identificado en la red `192.168.1.0`                                  |  `192.168.1.0`  |
|  Host Address   | Una dirección IP es utilizada para identificar a un dispositivo en una subred                                               | Un dispositivo puede tener la dirección de red `192.168.1.1`                                                         | `192.168.1.100` |
| Default Gateway | El Gateway predeterminado es una dirección especial asignada a un dispositivo que es capaz de enviar información a otra red | Cualquier dato que deba enviarse a un dispositivo que no se encuentre en la misma red se enviará a este dispositivo. | `192.168.1.254` |

> [!TIP]
> **Respecto al Gateway** \
> Pueden utilizar cualquier dirección de host pero por lo general utilizarán la primera o la última (`X.X.X.1` o `X.X.X.254`).

___
*Pregunta 1: What is the technical term for dividing a network up into smaller pieces?* \
**Respuesta: Subnettig**

> **NOTA**: El subnettig permite dividir una red muy grande

*Pregunta 2: How many **bits** are in a subnet mask?* \
**Respuesta: 32**

> **RECORDATORIO**: Los 32-bits están repartidos en los octetos.

*Pregunta 3: What is the range of a section (octet) of a subnet mask?* \
**Respuesta: 0-255**

*Pregunta 4: What address is used to identify the start of a network?* \
**Respuesta: Network address**

> **PALABRAS CLAVE** -> *start of a network*

*Pregunta 5: What address is used to identify devices within a network?* \
**Respuesta: Host address**

*Pregunta 6: What is the name used to identify the device responsible for sending data to another network?* \
**Respuesta: Default gateway**

### Task 3 — ARP
El Address Resolution Protocol (**ARP**) es el encargado de permitir vincular una dirección MAC con una dirección IP en la red.

> Cada dispositivo de la red mantiene un registro de las direcciones **MAC** asociadas a otros dispositivos de la misma red.

Cuando un dispositivo quiere comunicarse con otro en la red, envía un _broadcast_ a toda la red en busca del dispositivo.

> [!NOTE]
> **ARP** almacena en caché los identificadores de otros dispositivos en la red para poder comunicarse con ellos en otro momento.

| Paquete ARP     | ¿Qué hace?                                                                                                   |
| --------------- | ------------------------------------------------------------------------------------------------------------ |
| **ARP Request** | Lanza un mensaje a toda la red preguntando por la dirección MAC asociada a la IP por la que está preguntando |
| **ARP Reply**   | El dispositivo que tenga esa esa IP responderá con su propia dirección MAC                                   |

___
*Pregunta 1: What does ARP stand for?* \
**Respuesta: Address Resolution Protocol**

*Pregunta 2: What category of ARP Packet asks a device whether or not it has a specific IP address?* \
**Respuesta: Request**

> **PALABRA CLAVE** -> *asks*

*Pregunta 3: What address is used as a physical identifier for a device on a network?* \
**Respuesta: MAC Address**

*Pregunta 4: What address is used as a logical identifier for a device on a network?* \
**Respuesta: IP Address**

### Task 4 — DHCP

> [!NOTE]
> El **DHCP** (Dynamic Host Configuration Protocol) asigna automáticamente cuando un nuevo host es conectado a la red y no se le ha asignado una IP manualmente.

| Paquete DHCP      | ¿Qué hace?                                                                                    |
| :---------------: | --------------------------------------------------------------------------------------------- |
| **DHCP Discover** | El host manda el paquete para verificar que exista un servidor DHCP en la red                 |
| **DHCP Offer**    | El servidor DHCP devuelve la respuesta con una dirección IP que el dispositivo puede utilizar |
| **DHCP Request**  | El dispositivo confirma que va a utilizar la IP que el servidor le ofreció                    |
| **DHCP ACK**      | El servidor confirma el proceso y el dispositivo puede empezar a utilizar la dirección IP     |

___
*Pregunta 1: What type of DHCP packet is used by a device to _retrieve an IP address?_* \
**Respuesta: DHCP Discover**

*Pregunta 2: What type of DHCP packet does a device **send once it has been** **offered an IP address** by the DHCP server?* \
**Respuesta: DHCP Request**

*Pregunta 3: Finally, **what is the last** DHCP packet that is sent to a device from a DHCP server?* \
**Respuesta: DHCP ACK**

# Lecciones aprendidas
- La topología de red más común es la de tipo estrella pero también es la más cara.
- La topología de Bus es la más sencilla de implementar pero es la que mas desventajas tiene ya que si se daña el bus, toda la infraestructura se queda sin red.
- El enrutamiento hace una ruta segura para que los datos se envíen de una red a otra.
- El subnettig ayuda a crear redes más pequeñas dentro de una red mucho más grande.
- La dirección de red es tanto el inicio de una red como el identificador de la misma.
- El protocolo ARP es el encargado de vincular una dirección MAC con una dirección IP.
- El DHCP se encarga de asignar las direcciones IP a los dispositivos de una red
