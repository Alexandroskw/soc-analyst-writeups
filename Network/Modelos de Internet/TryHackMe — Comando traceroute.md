La continuación lógica del comando ping es `traceroute`. Internet está hecho por muchas diferentes servidores y endpoints conectados entre sí y antes de llegar al contenido que has solicitado, debes de saltar a través de un montón de otros servidores.
Traceroute te permite ver todos estas conexiones, es decir, te permite ver todos los saltos que hace tu petición hasta llegar al contenido que has pedido en primer lugar.
En Windows el comando es `tracert` y al igual que el comando `ping`, opera usando el mismo protocolo `ICMP`, mientras que en sistemas Unix y Unix-like, opera sobre `UDP`.

---
*Pregunta 1: Usar `traceroute` en tryhackme.com para ver la ruta de la petición*
**Respuesta: NO SE NECESITA RESPONDER**
*Pregunta 2: ¿Cuál bandera usarías para especificar una interfaz cuando utilizas `traceroute`?*
**Respuesta: -i**
*Pregunta 3: ¿Cuál bandera usarías si quisieras usar peticiones TCP SYN cuando trazas la ruta?*
**Respuesta: -T**
*Pregunta 4: (Pensamiento lateral) ¿En qué capa del modelo TCP/IP corre `traceroute` por defecto (Windows)?*
**Respuesta: Internet**

---
Para la última pregunta, en la explicación de la tarea se explica que en Windows, `tracert` opera bajo el protocolo `ICMP` mientras que en los sistemas Unix y Unix-like `traceroute` opera bajo el protocolo `UDP`. El protocolo UDP, al ser un protocolo de la capa de transporte, es de la capa de Transporte en el modelo TCP/IP y del modelo OSI, mientras que ICMP es un protocolo de la capa de Internet del modelo TCP/IP y en la capa de red del modelo OSI.