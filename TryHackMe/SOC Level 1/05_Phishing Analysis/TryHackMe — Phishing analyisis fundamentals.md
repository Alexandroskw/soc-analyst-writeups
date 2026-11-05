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
## Email Headers
+ **Header**: Contienen los metadatos del mensaje (como el emisor y los servidores involucrados)
+ **Body**: Es el contenido del mensaje (puede ser texto plano o HTML)

Cuando se revisa un email por su código fuente salen a la vista elementos que ayudan a entender la naturaleza del archivo adjunto:
- **Content-Type**: qué tipo de archivo es (`application/pdf`)
- **Content-Disposition**: indica que el archivo está adjunto y también pone su nombre
- **Content-Transfer-Encoding**: muestra el cifrado del archivo, por ejemplo base64

> El cifrado `base64` representa al archivo en sí mismo. Se puede reconstruir el archivo original con herramientas como CyberChef o un convertidor de base64 a PDF

## Lecciones aprendidas