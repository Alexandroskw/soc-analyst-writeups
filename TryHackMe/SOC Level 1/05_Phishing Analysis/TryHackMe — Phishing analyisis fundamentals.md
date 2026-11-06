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

### Phishing

| Nombre         | Descripción                                                                                                           |
| -------------- | --------------------------------------------------------------------------------------------------------------------- |
| Spam           | Emails masivos no solicitados a múltiples receptores.                                                                 |
| Phishing       | Emails que suplantan la identidad de una entidad fiable para engañar a los receptores y obtener información sensible. |
| Spear phishing | Una forma dirigida de phishing que apunta a un objetivo en específico dentro de la organización                       |
| Whaling        | Un spear phishing especial que apunta a los "peces gordos", como un CEO                                               |
| Smishing       | Un phishing que se lleva a cabo por mensajes de texto, solo se lleva a cabo en dispositivos móviles                   |
| Vishing        | Ataques de phishing que se llevan a cabo a través de llamadas de voz                                                  |
#### Anatomía del phishing

| Característica                      | Qué es                                                                                             |
| ----------------------------------- | -------------------------------------------------------------------------------------------------- |
| Falsificación de dirección          | El emisor falsifica la dirección de una entidad legítima                                           |
| Asunto/mensaje urgente              | Crea una sensación de urgencia ("You have 72 hours until your account will be deleted")            |
| Suplantación de marca               | Imita a una organización legítima                                                                  |
| Errores gramaticales y ortográficos | Gracias a la IA ahora son menos comunes pero ahora tienen frases extrañas o redacción poco natural |
| Contenido genérico                  | El mensaje carece de personalidad ("Dear custumer", "Hello")                                       |
| Enlaces ocultos o recortados        | Los hipervínculos no muestran la dirección destino                                                 |
| Archivos adjuntos maliciosos        | Los archivos adjuntos aparentan ser legítimos                                                      |

> El **Business Email Comprimise** es un tipo de ataque en el que un atacante obtiene acceso a la cuenta legítima de un empleado de la organización y la utiliza para engañar a otras personas para que realicen operaciones fraudulentas

## Lecciones aprendidas
- Es importante saber qué tipo de protocolo estamos utilizando en la bandeja de entrada
- Los headers de un email dicen más de lo que uno piensa. Es importante revisarlos con cautela y paciencia
- El `Content-Type` indica el tipo de archivo que está adjunto en un email
- Se puede reconstruir un archivo utilizando `base64` y convertirlo, por ejemplo, a un formato PDF para conocer su contenido
- Existen diversos tipos de phishing, unos más especializados que otros pero con la finalidad de engañar al receptor del mensaje

## Referencias
[Investigación de posible correo de phishing](Investigación%20de%20posible%20correo%20de%20phishing.md)