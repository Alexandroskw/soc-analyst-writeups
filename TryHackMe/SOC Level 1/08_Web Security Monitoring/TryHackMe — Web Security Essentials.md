# TryHackMe — Web Security Essentials
**Dificultad** -> easy | **Date** -> 23-nov-2026 | **Type** -> Free \
**Sala** -> [Web Security Essentials](https://www.tryhackme.com/room/websecurityessentials)

## Introducción
Sala enfocada en la comprensión de cómo funciona la red, sus vulnerabilidades más comunes y cómo mantenerse a salvo en Internet.

## Solución
### Task 2 — Why web?
Se ha estado cambiando de aplicaciones de escritorio a aplicaciones web desde hace décadas. En los años 90 la norma era que las apps fueran de escritorio debido a las limitaciones de la época pero al llegar a los 2000 todo cambió debido a que la tecnología web avanzó haciendo más dinámicas las apps de correo, redes sociales y banca. \
En la década del 2010 fue un aumento considerable en el uso de apps basadas en la nube y el Software as a Service (**Saas**).

#### Desde la seguridad

El uso de apps web trae ventajas como mejor compatibilidad, mayor accesibilidad, actualizaciones más rápidas y menor carga a la computadora del usuario.

> [!CAUTION]
> Entre más se expandan las apps web, más oportunidades hay para los atacantes. Las apps web son el punto de ataque más común para los atacantes ya que siempre están disponibles y expuestas.

> [!IMPORTANT]
> El propietario de la app web es el responsable completo de la seguridad de los datos de los usuarios.

### Task 3 — Web Infrastructure

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

### 

## Lecciones aprendidas