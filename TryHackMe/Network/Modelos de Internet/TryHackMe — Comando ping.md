Sirve para probar si es posible la conexión en un servidor remoto, es decir, si se puede conectar a un sitio web en internet aunque también para comprobar si una computadora en tu red ha sido configurada bien. Ping trabaja usando el protocolo `ICMP` que es mucho menos conocido que `TCP/IP`.
`ICMP` trabaja en la capa de red del modelo OSI y en la capa de internet del modelo TCP/IP.
___
> [!NOTA]
> Para las preguntas de las banderas, se utiliza el manual de `ping`
> (`man ping` en la terminal) y se puede utilizar la búsqueda con `/` y buscar la palabra clave.

___
*Pregunta 1: ¿Qué comando usarías para hacer ping al sitio web bbc.co.uk?*
**Respuesta: ping bbc.co.uk**

*Pregunta 2: Haz ping a muirlandoracle.co.uk. ¿Cuál es la dirección IPv4?*
**Respuesta: 217.160.0.152**

*Pregunta 3: ¿Qué bandera te permite cambiar el intervalo de solicitudes de ping enviadas?*
**Respuesta: -i**

*Pregunta 4: ¿Qué bandera te permite restringir las peticiones a IPv4?*
**Respuesta: -4**

*Pregunta 5: ¿Qué bandera te da una salida mas verbosa?*
**Respuesta: -v**
___
[TryHackMe — Comando traceroute](<TryHackMe — Comando traceroute.md>)
[TryHackMe — Comando WHOIS](<TryHackMe — Comando WHOIS.md>)
[TryHackMe — Comando dig](<TryHackMe — Comando dig.md>)