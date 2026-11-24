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
> Los logs son una herramienta valiosa para la detección de los logs ya que cada petición enviada al servidor web deja evidencia en los logs de acceso y los defensores pueden analizar estas entradas para revisar patrones que revelen escaneo, intentos de explotación u otro tipo de ataques.


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



## Lecciones aprendidas
