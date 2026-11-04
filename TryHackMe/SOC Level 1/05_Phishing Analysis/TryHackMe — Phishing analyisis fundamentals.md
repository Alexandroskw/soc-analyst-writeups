# TryHackMe — Phishing analysis fundamentals
**Dificultad** -> easy | **Date** -> 03-nov-2026 | **Type** -> Free \
**Sala** -> [Phishing analysis fundamentals](https://www.tryhackme.com/room/phishingemails1tryoe)

## Introducción
Sala enfocada en la identificación de un email de tipo phishing.
## Solución
### Email delivery
Al enviar un email varios protocolos trabajan en conjunto para enviar el email del emisor al receptor:
- **SMTP** (Simple Mail Transfer Protocol)
- **POP3** (Post Office Protocol 3): Descarga el email en el dispositivo
- **IMAP** (Internet Message Access Protocol): Sincroniza los emails a través de los dispositivos

| POP3                                                                                                                                                                                                                                                                                             | IMAP                                                                                                                                                                                                                                                                                |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| - Los correos son descargados y almacenados en un dispositivo<br>- Los mensajes enviados son almacenados en el dispositivo que los envió<br>- Solo se pueden accesar a los mensajes en el dispositivo que los envió<br>- Los mensajes típicamente son removidos del servidor cuando se descargan | - Los mensajes se almacenan en el servidor y se pueden descargar en múltiples dispositivos<br>- Los mensajes enviados son almacenados en el servidor<br>- Los mensajes se sincronizan en multiples plataformas<br>- Los mensajes permanecen en el servidor a no ser que se eliminen |

## Lecciones aprendidas