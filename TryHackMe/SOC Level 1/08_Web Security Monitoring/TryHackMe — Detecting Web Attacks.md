# TryHackMe — Detecting Web Attacks
**Dificultad** -> easy | **Date** -> 24-nov-2026 | **Type** -> Free \
**Sala** -> [Detecting Web Attacks](https://www.tryhackme.com/room/detectingwebattacks)

## Introducción
Sala enfocada en la exploración de los ataques y métodos de detección a través de los logs y el análisis del tráfico de la red.

## Solución
### Client-side attacks

Se basan en abusar de las debilidades en el comportamiento o dispositivo del usuario. En particular, los atacantes explotan las vulnerabilidades en los navegadores o engañan a los usuarios para realizar acciones inseguras para obtener acceso a sus cuentas y robar información sensible.

> [!NOTE]
> Debido al alza en las las aplicaciones web, las compañías están comenzando a integrar plugins de terceros con lo cual aumenta el ataque a superficie en los navegadores.

> [!IMPORTANT]
> Las herramientas para un analista (logs del lado del servidor o captura del tráfico de red), ofrecen muy poca o ninguna información sobre lo que ocurre en el navegador del usuario.

Ataques comunes del lado del cliente:

- **Cross-Site Scripting (XSS)**: El más común. Un script malicioso en un sitio web confiable es ejecutado en el navegador del usuario. Una caja de texto no tiene filtros de entrada un atacante puede introducir una cadena: `Hello <script>alert('You have been hacked);</script>` 
- **Cross-Site Request Forgery (CSRF)**: El navegador es engañado al enviar una solicitud no autorizada en nombre de un usuario legítimo
- **Clickhijacking**: Los atacantes sobreponen elementos invisibles al inicio del contenido legítimo haciendo que los usuarios crean que están interactuando con contenido seguro

## Lecciones aprendidas
