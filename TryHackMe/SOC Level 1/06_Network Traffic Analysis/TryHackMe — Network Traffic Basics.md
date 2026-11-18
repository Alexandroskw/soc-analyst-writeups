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

## Lecciones aprendidas