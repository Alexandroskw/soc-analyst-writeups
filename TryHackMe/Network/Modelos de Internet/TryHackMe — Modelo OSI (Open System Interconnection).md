Actualmente el Modelo OSI es más referencial que algo usado en el internet moderno
## Capa 7 — Aplicación (Application)
La aplicación en si, lo que vemos en la pantalla, por ejemplo al buscar en el navegador Google o Facebook.
![](<Pasted image 20260720173921.png>)
En esta capa viven el protocolo `HTTP`/`HTTPS` y `SMTP`. Esta capa se interactua directamente con los datos del usuario.
## Capa 6 — Presentación (Presentation)
Es la encargada del cifrado de caracteres, cifrado de la aplicación. Es responsable de preparar los datos para que los pueda usar la capa de aplicación. Aquí vive el cifrado `SSL/TLS`.
![](<Pasted image 20260720174159.png>)
Es la responsable de traducir los datos entrantes en una sintaxis que la capa de aplicación del dispositivo receptor pueda comprender.
## Capa 5 — Sesión (Session)
Se encarga de establecer conexión entre dispositivos.
![](<Pasted image 20260720174456.png>)
Garantiza que la sesión permanezca abierta el tiempo suficiente como para transferir todos los datos que se están intercambiando. Crea puntos de control si se está descargando un archivo, la capa fija un punto de control cada 5 megabytes. Si se interrumpe la descarga, se reinicia en el último punto de control.
## Capa 4 — Transporte (Transport)
Se encarga de, valga la redundancia, transportar la información. Aquí viven los protocolos `TCP` y `UDP`. En esta capa ocurre el famoso **SYN, SYN-ACK, ACK** 3 way handshake. Es la encargada del control del flujo y de errores pero dentro de la red
![](<Pasted image 20260720180257.png>)
Antes de proceder a ejecutar el envío a la capa 3, tomar datos de la capa de sesión y fragmentarlos seguidamente en trozos más pequeños llamados **segmentos**, esto último si se escoge el protocolo `TCP`. Si se escoge el protocolo `UDP`, esos trozos se llamarán **datagramas**.
## Capa 3 — Red (Network)
El róuter es el principal dispositivo en esta capa. Es el encargado "enrutar" (de ahí el nombre), el tráfico de toda la red. Busca la mejor ruta física para que los datos lleguen a su destino
![](<Pasted image 20260720180630.png>)
La dirección IP vive aquí, también los paquetes en sí. Aquí es donde los diferentes frames atraviesan diferentes redes.
Si los dispositivos que se comunican se encuentran en la misma red, entonces la capa de red no es necesaria.
## Capa 2 — Enlace de datos (Data link)
La que enlaza con la capa de red. La MAC vive en esta capa y los protocolos control de enlace de datos (Data Link Control) (DLC). El dispositivo principal es el switch. Si la transferencia entre dos dispositivos es en la misma red, todo va sobre esta capa.
![](<Pasted image 20260720181240.png>)
Facilita la transferencia de datos entre dos dispositivos dentro la _misma_ red, a diferencia de la capa 3. Es la que se encarga de revisar la integridad de los datos que recibe de la capa 1.
## Capa 1 — Física (Physical)
Señales, cables, conectores. No hay ningún protocolo, es el hardware y la señal.
![](<Pasted image 20260720181425.png>)

### Mnemotecnia para recordar el modelo (en inglés)
> **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing

La letra inicial de cada una de las palabras representa una de las capas del modelo de arriba hacia abajo.
[TryHackMe — Modelo TCP-IP](<TryHackMe — Modelo TCP-IP.md>)[TryHackMe — Comando ping](<TryHackMe — Comando ping.md>)