# TryHackMe — Web Security Essentials
**Dificultad** -> easy | **Date** -> 23-nov-2026 | **Type** -> Free \
**Sala** -> [Web Security Essentials](https://www.tryhackme.com/room/websecurityessentials)

## Introducción
Sala enfocada en la comprensión de cómo funciona la red, sus vulnerabilidades más comunes y cómo mantenerse a salvo en Internet.

## Solución
### Why web?
Se ha estado cambiando de aplicaciones de escritorio a aplicaciones web desde hace décadas. En los años 90 la norma era que las apps fueran de escritorio debido a las limitaciones de la época pero al llegar a los 2000 todo cambió debido a que la tecnología web avanzó haciendo más dinámicas las apps de correo, redes sociales y banca. \
En la década del 2010 fue un aumento considerable en el uso de apps basadas en la nube y el Software as a Service (**Saas**).

#### Desde la seguridad

El uso de apps web trae ventajas como mejor compatibilidad, mayor accesibilidad, actualizaciones más rápidas y menor carga a la computadora del usuario.

> [!CAUTION]
> Entre más se expandan las apps web, más oportunidades hay para los atacantes. Las apps web son el punto de ataque más común para los atacantes ya que siempre están disponibles y expuestas.

> [!IMPORTANT]
> El propietario de la app web es el responsable completo de la seguridad de los datos de los usuarios.

### Web Infrastructure

> [!NOTE]
> Los atacantes pueden utilizar el ciclo de petición-respuesta (request-response) para saturar el servidor y brincarse los controles de seguridad o para engañar al servidor y ejecutar comandos maliciosos.

#### Componentes de un servicio web

| Componente                   | ¿Qué contiene                                                                     |
| ---------------------------- | --------------------------------------------------------------------------------- |
| Aplicación (**Application**) | El código, imágenes, estilos e iconos para que la app funcione y se vea bien      |
| Servidor (**Web Server**)    | Almacena la aplicación, escucha las peticiones y devuelve la respuesta al usuario |
| Cliente (**Host Machine**)   | El Sistema Operativo (Linux o Windows) que ejecuta la app web y el servidor       |

#### Tipos de servidores

| Nombre                                  | Descripción                                                                                                     |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Apache                                  | El más popular. Almacena sitios web comúnes y blogs como WordPress                                              |
| NGINX                                   | El estándar de a nivel industrial para apps de alto desempeño. Lo utilizan sitios como Netflix, AirBnB o GitHub |
| Internet Information Services (**IIS**) | Es desarrollado por Microsoft. Se usa en entornos empresariales                                                 |

> [!IMPORTANT]
> Los servidores web están posicionados en frente de las apps y sitios web, por eso son una parte fundamental de la infraestructura de Internet. Debido a ello, (y que están expuestos) son un blanco común para ataques.

### Protecting the web

> Hay varias medidas de seguridad disponibles para asegurar apps y sitios web. A estas medidas se le conocen como **Mitigación** ya que algunas de ellas proveen visibilidad y otras pueden detener activamente o limitar un ataque.

| Componente a asegurar       | Descripción                                                                                                                                                                                                                                                                                            |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Protegiendo la Aplicación   | - Escribir código seguro, evitando funciones inseguras manejando errores correctamente y eliminando información sensible.<br>- Validación y saneamiento de las entradas previniendo ataques de inyecciones como XSS.<br>- Controles de acceso restringiendo el acceso a ciertos usuarios               |
| Protegiendo el servidor web | - Mantener un registro detallado de todas las peticiones con los logs de acceso<br>- Filtrar y bloquear el tráfico a través de un firewall (Web Application Firewall)<br>- Reducir la exposición directa del servidor y utilizar WAFs centralizando el contenido de la red (Content Delilvery Network) |
| Protegiendo al Host         | - Usar privilegios de usuario para los servicios<br>- Desactivar servicios innecesarios y cerrar puertos sin usar<br>- Agregar un antivirus a nivel de endpoint que bloquee malware conocido                                                                                                           |
| Protección general          | - No permitir acceso libre al código, paneles de administración o al host<br>- Asegurar que las dependencias del servidor, servidor web y el host estén al día                                                                                                                                         |

> [!IMPORTANT]
> Los servidores web crean registros de cada petición que recibe. Estos logs son valiosos ya que registran información acerca de cada interacción que tiene el servidor, incluyendo la IP, marca de tiempo, datos o páginas solicitadas, respuestas del estado del servidor y el User-Agent

### Defense Systems

> [!TIP]
> **Sobre los CDN (Content Delivery Network)** \
> Almacenan y entregan contenido en caché de servidores cercanos para reducir la latencia a los usuarios. También ayudan a la seguridad actuando como búfer entre el usuario y el servidor original.


| Beneficio             | ¿Qué hace?                                                                                 |
| --------------------- | ------------------------------------------------------------------------------------------ |
| Enmascaramiento de IP | Esconde la IP original del servidor, haciendo más difícil al atacante apuntar a el         |
| Protección DDoS       | Pueden absorber una gran cantidad de tráfico haciendo que el ataque DoS sea menos efectivo |
| Forzar HTTPS          | Cifrar la comunicación a través de TLS (obligatorio en la mayoría de los CDN)              |
| WAF integrado         | Muchos CNDs integran firewalls de apps web                                                 |

#### Web Application Firewall (WAF)

> [!TIP]
> Inspeccionan el tráfico HTTP entrante y lo bloquean (o registran) basados en reglas de seguridad.

Tipo de WAF disponibles:
- **Basado en cloud (Cloud-based) (Reverse proxy)**: Se sitúa en frente del servidor web
- **Basado en host (host-based)**: Software que se instala en el servidor web y permite el control de cada aplicación
- **Basado en red (network-based)**: Es un dispositivo físico o virtual situado en el perímetro de la red

| Característica                                   | Método de detección                                                | Ejemplo                                                                                   |
| ------------------------------------------------ | ------------------------------------------------------------------ | ----------------------------------------------------------------------------------------- |
| Detección basada en firma (signature-based)      | Empata patrones de ataque comunes o payloads                       | Una solicitud con un `User-Agent` que coincide con la herramienta `sqlmap/1.8.1`          |
| Detección basada en heurística (Heuristic-based) | Analiza el contexto y el comportamiento de la solicitud            | Una cadena de consulta con caracteres especiales (`search?q=%3Cscript%20(1)`)             |
| Análisis de anomalías y comportamiento           | Marca las desviaciones del comportamiento del tráfico habitual     | Una sola IP haciendo repetidos intentos de inicio de sesión en un corto periodo de tiempo |
| Filtrado por locación y reputación de IP         | Usa la localización e inteligencia de amenazas para bloquear la IP | Una solicitud de una IP que se encuentra fuera del área de operaciones usual              |

> [!NOTE]
> Los métodos de detección están en constante evolución, dar una tabla más grande sería virtualmente imposible. Las reglas de detección pueden ser personalizadas también, basadas en las necesidades del dueño de la aplicación.

### Antivirus (AV)

> La mayoría de los antivirus se basan en la detección de firmas (signature-based), comparando los archivos con una base de datos patrones o malware conocido.

> [!TIP]
> Los AV son solo una capa de seguridad más, se deben de combinar con otros elementos de seguridad para dar una mejor protección.

## Lecciones aprendidas
- La migración de aplicaciones de escritorio a aplicaciónes web no es nuevo, lleva al menos tres décadas en marcha
- Las apps webs aportan ventajas al usuario final y al mismo tiempo dan una serie de brechas de seguridad
- Entre más crezcan las apps web, mas superficie de ataque existe
- Los propietarios de las apps son los responsables directos de la seguridad de los datos de los usuarios de sus aplicaciones
- Las partes principales de un servidor web son: **aplicación**, **servidor web** y el **host**
- Los servidores mas conocidos son Apache y NGINX. Existe el IIS de Microsoft que es usado en entornos empresariales
- Los servidores web crean sus propios logs que registran toda la interacción entre el usuario y el servidor que posteriormente se puede utilizar para identificar actividad maliciosa
- Los WAF son la primera línea de defensa de los servidores web y son los encargados de defenderlos en contra de ataques
	- Basados en web
	- Basados en firmas
	- Basados en host
- Los WAFs tienen ciertas características para detectar al malware y comportamientos anómalos en el servidor
- Las mitigaciones previenen de algunos ataques pero no los eliminan del todo
- Existen más métodos de detección puesto que están siempre en constante evolución
- Los CDN son una capa más de defensa y en algunos casos pueden absorber ataques como los DoS
- Los CDN sirven como caché para que los usuarios no tengan latencia al usar la aplicación