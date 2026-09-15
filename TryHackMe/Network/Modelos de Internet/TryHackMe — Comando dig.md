En el nivel más básico, el **DNS** nos permite preguntar por un servidor especial que nos proporcione la dirección IP del sitio web que estamos intentando accesar. Por ejemplo, si hacemos una solicitud a google.com, nuestra computadora primero enviaría una petición al servidor DNS. El servidor entonces revisaría por la IP de Google y la enviaría de vuelta a nuestra computadora. En otras palabras, es como un directorio telefónico, donde buscas el nombre de la persona en lugar del número telefónico.
## Servidor recursivo
Son gestionados comúnmente por el ISP (Internet Service Provider) aunque también por las propias organizaciones. Los servidores recursivos actúan en nombre del usuario final para convertir el nombre de dominio en la dirección IP. También almacenan en caché las respuestas de una solicitud durante un periodo de tiempo específico.

---
*Pregunta 1: ¿Qué significa DNS?*
**Respuesta: Domain Name System**
*Pregunta 2: ¿Cuál es el primer tipo de servidor DNS que tu computadora deberá consultar cuando busques por un dominio?*
**Respuesta: Recursive**
*Pregunta 3: ¿Qué tipo de servidor DNS contiene registros específicos para extensiones de dominio (.com, .co.uk, etc)? Usa la versión larga del nombre*
**Respuesta: Top-Level Domain**
*Pregunta 4: ¿Dónde es el primer lugar que tu computadora deberá buscar para encontrar la dirección IP de un dominio?*
**Respuesta: Hosts File**
*Pregunta 5: (Búsqueda) Google corre sobre dos DNS públicos. Uno de ellos puede ser consultado con la IP 8.8.8.8, ¿cuál es la dirección IP para el otro?*
**Respuesta: 8.8.4.4**
*Pregunta 6: Si una consulta DNS tiene un TTL de 24 horas, ¿Qué número deberá mostrar la consulta dig?*
**Respuesta: 86400**

---
Para la pregunta 5, basta con buscar en Google (o tu buscador favorito), los DNS públicos que tiene Google.
Para la pregunta 6, es similar, es mejor buscar cuántos segundos tiene un día o 24 horas.