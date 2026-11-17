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
## Lecciones aprendidas