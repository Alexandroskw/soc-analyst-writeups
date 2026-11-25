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

##### Ataques comunes del lado del cliente:

- **Cross-Site Scripting (XSS)**: El más común. Un script malicioso en un sitio web confiable es ejecutado en el navegador del usuario. Una caja de texto no tiene filtros de entrada un atacante puede introducir una cadena: `Hello <script>alert('You have been hacked);</script>` 
- **Cross-Site Request Forgery (CSRF)**: El navegador es engañado al enviar una solicitud no autorizada en nombre de un usuario legítimo
- **Clickhijacking**: Los atacantes sobreponen elementos invisibles al inicio del contenido legítimo haciendo que los usuarios crean que están interactuando con contenido seguro

### Server-side attacks
Se basan en explotar las vulnerabilidades dentro del servidor web, el código de la app o el backend que soporta al sitio o app web. Se enfocan en tomar ventaja de las propias vulnerabilidades del sistema. 

> [!NOTE]
> Los atacantes pueden ganar acceso aprovechándose de las fallas y de las vulnerabilidades, lógica del servidor, malas configuraciones o el manejo de entradas y también causar daño a los servicios en ejecución.

> [!IMPORTANT]
> Cada petición web enviada a una aplicación es procesada por el servidor y es registrada en los logs u otro sistema de monitoreo; estas peticiones viajan a través de la red, lo que significa que un análisis puede revelar comportamientos sospechosos.

##### Ataque comunes del lado del servidor
- **Fuerza bruta (brute-force)**: ocurre cuando un atacante intenta diferentes nombres de usuarios o contraseñas repetidamente para obtener acceso.
- **SQL Injection (SQLi)**: Es una ataque a la base de datos que está detrás del sitio o aplicación web. El atacante construye queries a través de concatenación de cadenas en lugar de usar queries parametrizadas.
- **Command Injection**: El sitio web toma la entrada del usuario y la pasa al sistema sin revisarla permitiendo que los atacantes puedan mandar comandos de manera encubierta para que el servidor los ejecute con el mismo privilegio que el de la aplicación.

### Log-based detection

> [!TIP]
> Los logs son una herramienta valiosa para la detección de los registros ya que cada petición enviada al servidor web deja evidencia en los logs de acceso y los defensores pueden analizar estas entradas para revisar patrones que revelen escaneo, intentos de explotación u otro tipo de ataques.

| Campo                                  | Ejemplo                                                                                |
| -------------------------------------- | -------------------------------------------------------------------------------------- |
| IP del cliente                         | Conocida como maliciosa o fuera del rango geográfico especificado                      |
| Marca de tiempo y la página solicitada | Las peticiones hechas en horas inusuales o repetidas en un periodo de tiempo muy corto |
| Código de estado                       | Respuestas `404` repetidas que indican que una página no pudo ser encontrada           |
| Tamaño de la respuesta                 | Significativamente grande o pequeña de lo normal a las respuestas usuales              |
| Referrer                               | Refiriendose a páginas que no encajan con la navegación habitual del sitio             |
| User-Agent                             | Navegador desactualizado o herramientas de ataque comunes (`sqlmap` o `wpscan`)        |

```
10.10.10.10 [20/aug/2025:17:43:01] "GET /profile.html HTTP/1.1" 200 2212 "/login.html" "Mozilla/5.0"
```

> [!NOTE]
> A pesar de ser muy útiles, los logs no siempre capturan todo el contenido de una petición, sobre todo el body de las peticiones `POST` y `GET`

> [!TIP]
> Los logs de acceso no registran todos los datos del método `POST`, así que los investigadores pueden ver qué petición está ocurriendo. Del mismo modo las peticiones `GET` pueden registrar las rutas completas y las cadenas de las queries pero algunos formatos no las incluyen del todo. Depende enteramente del software del servidor y de la configuración de los registros.

### Network-based detection

> [!TIP]
> El Network Traffic Analysis permite a un analista examinar los datos sin procesar entre el cliente y el servidor. Inspeccionando los paquetes un analista puede observar el comportamiento de un ataque en un nivel más detallado (incluyendo los protocolos de transporte y los datos de la aplicación en sí).

> [!NOTE]
> La captura de paquetes de red son mucho mas verbosas que los registros de los servidores pero revelan datos entre cada petición y respuesta como los headers HTTP, los bodies de la petición `POST`, cookies y los archivos cargados y descargados.

---

*Pregunta 1: What password does the attacker successfully identify in the brute-force attack?* \
**Respuesta: `astrongpassword123`**

> **NOTA** -> utilizar el filtro `http.response.code`

*Pregunta 2: What is the flag the attacker found in the database using SQLi?* \
**Respuesta: `THM{dumped_the_db}`**

> **TIP** -> Revisar los métodos `GET /account`

### Web application firewall (WAF)

> [!NOTE]
> Son la primera línea de defensa de una página o aplicación web. Analizan los paquetes completamente y descifran el tráfico TLS y lo filtra antes de que llegue al servidor.

#### Reglas

| Tipo                                                    | Descripción                                                                                                     | Ejemplo                                                         |
| ------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| Bloquear patrones comunes de ataque                     | Bloquea las payloads maliciosas conocidas                                                                       | Bloquear el User-Agent: `sqlmap`                                |
| Denegar fuentes maliciosas conocidas                    | Usando la reputación de una IP, inteligencia de amenazas o bloqueo geográfico para detener el tráfico peligroso | Bloquea IPs de campañas recientes de botnets                    |
| Reglas personalizadas                                   | Adaptadas a las necesidades de la aplicación específica                                                         | Permitir únicamente peticiones `GET` y `POST` en `/login`       |
| Limitar la tasa de transferencia y prevención de abusos | Limita la frecuencia de las peticiones para prevenir abusos                                                     | Limitar intentos de inicio de sesión a 5 por minuto por cada IP |

> [!NOTE]
> Actualmente los WAF modernos bloquean y detectan automáticamente User-Agents maliciosos.

Los WAF no siempre deben de bloquear el tráfico sospechoso, pueden utilizar CAPTCHA para verificar si es tráfico real o un bot malicioso.

> [!IMPORTANT]
> En la actualidad el tráfico de bots maliciosos ha incrementado un 37% de todo el tráfico global de la web. Por eso la importancia de que en los WAF se utilicen métodos de verificación como los CAPTCHA.

> [!IMPORTANT]
> Los WAF modernos incluyen reglas ya integradas diseñadas para mitigar los 10 principales riesgos de seguridad OWASP. También aprovechan las fuentes de inteligencia de amenazas para bloquear automáticamente peticiones de IPs maliciosa conocidas y User-Agents maliciosos.

## Lecciones aprendidas
- Los ataques del lado del cliente se aprovechan del usuario y el cómo utiliza su dispositivo y sobre todo el navegador que utiliza.
- La superficie de ataque a las aplicaciones web ha aumentado debido al aumento de plugins de terceros por el crecimiento de las primeras
- Lo que ocurre dentro del navegador es virtualmente imposible de detectar para un analista.
- Los ataques del lado del servidor aprovechan vulnerabilidades dentro del servidor y qué tan bien configurado está.
- Todas las peticiones que se hacen al servidor son registradas, por lo que un analista puede verificar si hubo un comportamiento inusual en el tráfico.
- Ya que se registra toda interacción entre el usuario y el servidor, un atacante puede dejar rastros de su ataque.
- A pesar de que se registra toda interacción, lo que no se registra es el contenido de las peticiones `GET` y `POST`.
- Wireshark permite analizar los datos de los paquetes entre el cliente y el servidor.
- La captura de los paquetes con Wireshark es mucho más detallada que con los logs.
- Los WAF son la primera línea. Ellos pueden detectar todo el tráfico e incluso descifrar el tráfico TLS (HTTPS).
- Los WAF modernos hacen los desafíos de CAPTCHA tan populares actualmente.
- El tráfico de bots en la red ha aumentado en un 37%.
- Los WAF pueden bloquear automáticamente User-Agents maliciosos conocidos.