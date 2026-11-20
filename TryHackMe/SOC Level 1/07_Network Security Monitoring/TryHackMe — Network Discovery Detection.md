# TryHackMe — Network Discovery Detection
**Dificultad** -> medium | **Date** -> 19-nov-2026 | **Type** -> Free \
**Sala** -> [Network Discovery Detection](https://www.tryhackme.com/room/networkdiscoverydetection)

## Introducción
Sala enfocada a la comprensión de como un atacante descubre activos en la red y como detectar dicha actividad.

## Solución
### Network Discovery
#### El atacante

> [!NOTE]
> Un adversario inicia su ataque tratando de descubrir activos en la organización que están al descubierto en el Internet, se le conoce como **Attack Surface** (_Ataque a superficie_)

En la fase de descubrimiento un atacante intenta obtener:
- Activos a los que se puede acceder
- IPs, puertos, Sistemas operativos y servicios corren en estos activos
- Versiones de los servicios en ejecución y si alguno de estos servicios tiene una vulnerabilidad para ser explotada

#### El defensor

> En ocasiones los defensores ejecutan software que realiza actividades de detección de redes

Durante la detección, los defensores quieren alcanzar los siguientes objetivos:
- Un inventario de los activos de la organización para asegurarse que están documetados
- Asegurarse que IPs, puertos o servicios innecesarios están abiertos y lo necesario está e ejecución
- Asegurarse que las vulnerabilidades están parchadas o al menos las vulnerabilidades que pueden ser explotadas lo estén

> [!IMPORTANT]
> Tanto atacantes como defensores realizan acciones de descubrimiento. El equipo SOC debe de distinguir entre el descubrimiento benigno del dañino.

Para poder distinguir entre un descubrimiento válido del no válido, los equipos SOC disponen de las siguientes técnicas:
- Incluir una lista de escáneres internos y externos permitidos asegurando que las alertas no se lancen en estas fuentes
- Integrar Inteligencia de Amenazas con la detección de casos de uso y marcar la actividad de escaneo solo de las fuentes maliciosas o sospechosas
- Se pudo haber pasado por alto alguna actividad de escaneo maliciosa, debido a ello algunos equipos utilizan la Inteligencia de Amenazas para elevar el nivel de severidad de las alertas en lugar de solo lanzarlas

### Escaneo externo vs interno
#### Escaneo externo
Un analista SOC puede observar que la IP de origen en este tipo de ataques es una IP externa y que la de destino es una IP que pertenece a la organización. Estos escaneos indican que el ataque sigue en la fase de reconocimiento de acuerdo con **MITRE ATT&CK** (es una alerta de baja severidad).

> El atacante aun no tiene un punto de apoyo, sigue haciendo el reconocimiento inicial para identificar alguna oportunidad que le dé acceso inicial a la red.

> [!TIP]
> El analista SOC puede bloquear la IP atacante a través del firewall del perímetro. Sin embargo se debe recordar que el atacante puede regresar ocultando su IP.

#### Escaneo interno
El analista SOC puede observar que la IP de origen y destino se encuentran dentro de la red de la organización. Estos escaneos indican que el atacante ahora está en la fase de descubrimiento de acuerdo con **MITRE ATT&CK** (es una alerta de alta severidad).

> En otros frameworks también se le conoce como reconocimiento interno, lo que indica que el atacante obtuvo un punto de apoyo en la red y ahora ha realizado un movimiento lateral.

> [!IMPORTANT]
> Después de confirmar que no es una actividad autorizada, el analista SOC puede escalar la alerta para iniciar el proceso de Respuesta a Incidentes.

### Escaneo vertical vs. horizontal

> [!IMPORTANT]
> **Escaneo horizontal** \
> Es un tipo de escaneo de puertos donde el atacante busca el mismo puerto a través de múltiples host en la red que está escaneando. \
> Se puede detectar si en los logs aparece la misma IP de origen en un mismo puerto pero en múltiples IPs.

> [!IMPORTANT]
> **Escaneo vertical** \
> Es un tipo de escaneo de puertos donde un solo host es escaneado a través de múltiples puertos. Los atacantes pueden realizar este escaneo para obtener un punto de apoyo en un host e identificar sus servicios expuestos. \
> Se puede detectar si en los logs se observa la misma IP de origen en la misma IP de destino pero en múltiples puertos de destino.

> En ocasiones los atacantes pueden realizar ambos tipos de escaneo para ganar ambas ventajas.

### Mecanismos de escaneo

| Ping Sweep                                                                                                                                                                                           | Escaneo TCP SYN                                                                                                                                                                                                                         | Escaneo UDP                                                                                                                                                                                                                            |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| El más básico de los escaneos. Envía un paquete **ICMP** al host objetivo. Si se encuentra en línea, responderá con su propio paquete **ICMP**. Es bloqueado por controles de seguridad actualmente. | El escaner envía una petición `SYN` al receptor, si responde con `SYN-ACK` significa que el host está en línea y el puerto donde el `SYN` fue enviado también. Es un escaneo sigiloso que usualmente se mezcla con el tráfico habitual. | Si el puerto está cerrado, el host envía un paquete ICMP con la respuesta de *Unreachable*, pero el host está en línea. Si no se recibe ninguna respuesta, el escaner marcará el puerto como abierto aunque no haya evidencia de ello. |

> [!NOTE]
> **Sobre el Escaneo UDP** \
> En algunos casos extraños, el escaneo puede recibir un paquete UDP en respuesta lo que significa que el puerto está abierto, sin embargo, este escaneo no es confiable y es lento ya que depende de que haya respuesta o se termine el tiempo de espera.

## Lecciones aprendidas
- El descubrimiento de redes es una parte crucial para que los atacantes consigan un punto de apoyo en la red, se le conoce como **Surface Attack**.
- El trabajo de un analista SOC en este contexto, es descubrir qué tráfico es benigno y qué trafico no lo es.
- El ataque a superficie se encarga de detectar los puntos débiles en un objetivo y explotar las vulnerabilidades encontradas.
- El escaneo externo es la fase de reconocimiento (de acuerdo con **MITRE ATT&CK**) de un atacante, aun sigue intentando descubrir qué vulnerabilidades existen
- El escaneo externo es la fase de descubrimiento (de acuerdo con **MITRE ATT&CK**) de un atacante, ya ha conseguido un punto de apoyo dentro de la red y está buscando información útil.
- El escaneo horizontal se basa en escanear el mismo puerto a través de la múltiples IPs disponibles en la red.
- El escaneo vertical se basa en escanear la misma IP a través de múltiples puertos.
- El escaneo más básico de todos es el **Ping Sweep** y al mismo tiempo es el más bloqueado.
- El Escaneo TCP también es conocido como escaneo sigiloso ya que usualmente se mezcla con el tráfico habitual de la red.
- El escaneo UDP es el escaneo más lento de todos ya que espera una respuesta o que el tiempo de conexión se agote.