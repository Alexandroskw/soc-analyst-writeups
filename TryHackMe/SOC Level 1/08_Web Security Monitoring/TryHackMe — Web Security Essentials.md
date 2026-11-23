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

#### Componentes de un servidor

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

### Protegiendo la red

> Hay varias medidas de seguridad disponibles para asegurar apps y sitios web. A estas medidas se le conocen como **Mitigación** ya que algunas de ellas proveen visibilidad y otras pueden detener activamente o limitar un ataque.

| Componente a asegurar       | Descripción                                                                                                                                                                                                                                                                                            |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Protegiendo la Aplicación   | - Escribir código seguro, evitando funciones inseguras manejando errores correctamente y eliminando información sensible.<br>- Validación y saneamiento de las entradas previniendo ataques de inyecciones como XSS.<br>- Controles de acceso restringiendo el acceso a ciertos usuarios               |
| Protegiendo el servidor web | - Mantener un registro detallado de todas las peticiones con los logs de acceso<br>- Filtrar y bloquear el tráfico a través de un firewall (Web Application Firewall)<br>- Reducir la exposición directa del servidor y utilizar WAFs centralizando el contenido de la red (Content Delilvery Network) |
| Protegiendo al Host         | - Usar privilegios de usuario para los servicios<br>- Desactivar servicios innecesarios y cerrar puertos sin usar<br>- Agregar un antivirus a nivel de endpoint que bloquee malware conocido                                                                                                           |
| Protección general          | - No permitir acceso libre al código, paneles de administración o al host<br>- Asegurar que las dependencias del servidor, servidor web y el host estén al día                                                                                                                                         |

> [!IMPORTANT]
> Los servidores web crean registros de cada petición que recibe. Estos logs son valiosos ya que registran información acerca de cada interacción que tiene el servidor, incluyendo la IP, marca de tiempo, datos o páginas solicitadas, respuestas del estado del servidor y el User-Agent

### 

## Lecciones aprendidas