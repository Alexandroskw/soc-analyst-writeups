Es como el modelo **OSI** pero un poco más antiguo. Además sirve como la base para las redes modernas en el mundo real. Consiste en 4 capas: 
1. **Aplicación** (**Application**)
2. **Transporte** (**Transport**)
3. **Internet**
4. **Interfaz de Red** (**Network Interface**)
> [!NOTA]
> Algunas fuentes separan el modelo en 5 capas, separando la capa 4 en Enlace de datos o Física como en el modelo OSI. Depende de cada quien qué modelo usar.

![[Modelo OSI vs Modelo TCP-IP.png]]La encapsulación y desencapsulación trabaja de la misma forma en el modelo TCP/IP que en el modelo OSI. Un header se agrega en la encapsulación y se remueve durante la desencapsulación.
El Modelo TCP/IP toma su nombre de dos de los protocolos más importantes de este modelo: el **T**ransmission **C**ontrol **P**rotocol (`TCP`) que controla el flujo de datos entre dos dispositivos y el **I**nternet **P**rotocol (`IP`) que controla como es que los paquetes son direccionados y enviados.
`TCP` es un protocolo basado en conexión, lo que quiere decir que antes de que se envíe cualquier dato vía TCP, se debe asegurar una conexión estable entre dos computadoras; a este proceso de formación esta conexión se le llama **Three-way Handshake**.
Cuando se intenta establecer una conexión, tu computadora manda primero un bit especial llamado `SYN` que establece el primer contacto iniciando la conexión. Despues el servidor responderá con el mismo bit `SYN` y otro más llamado `ACK`. Finalmente tu computadora enviará un paquete de vuelta que contiene `ACK` confirmando que la conexión se ha establecido.

--- 
*Pregunta 1: ¿Cuál modelo fue introducido primero, OSI o TCP/IP*
**Respuesta: TCP/IP**
*Pregunta 2: ¿Cuál capa del modelo TCP/IP cubre la funcionalidad de la Capa de Transporte del modelo OSI?*
**Respuesta: Transport**
*Pregunta 3: ¿Cuál capa del modelo TCP/IP cubre la funcionalidad de la Capa de Sesión del modelo OSI?*
**Respuesta: Application**
*Pregunta 4: La Capa de Interfaz de Red del modelo TCP/IP cubre la funcionalidad de dos capas del modelo OSI. Estas capas son Enlace de Datos, ¿y?...*
**Respuesta: Physical**
*Pregunta 5: ¿Cuál capa del modelo TCP/IP maneja la funcionalidad de la capa de Red del modelo OSI?*
**Respuesta: Internet**
*Pregunta 6: ¿Qué tipo de protocolo es TCP?*
**Respuesta: Connection-based**
*Pregunta 7: ¿Qué abreviación es la de SYN?*
**Respuesta: Synchronise**
*Pregunta 8: ¿Cuál es el segundo paso del Three-Way Handshake?*
**Respuesta: SYN/ACK**
*Pregunta 9: ¿Cuál es la abreviación del segmento "Acknowledgement" en el Three-Way Handshake?*
**Respuesta: ACK**
[[TryHackMe — Comando ping]]