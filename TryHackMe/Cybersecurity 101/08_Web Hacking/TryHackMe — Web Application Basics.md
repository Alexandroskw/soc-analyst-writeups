# TryHackMe — Web Application Basics
**Dificultad** -> easy | **Date** -> 06-oct-26 | **Type** -> Free \
**Sala** -> [Web Application Basics](https://www.tryhackme.com/room/webapplicationbasics)

## Introducción
La sala se enfoca en lo elemental de una aplicación web como una URL, peticiones y respuestas HTTP.

## Solución
### Task 2 — Web Application Overview
Se puede pensar en una aplicación web como un planeta, donde los internautas son los astronautas que exploran el planeta por la superficie a miles de kilómetros de distancia viendo únicamente lo que hay en la superficie.

| Componente | Partes                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|:----------:| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Front-end  | Es la superficie del planeta, con lo que el usuario puede interactuar como plantas y animales<br>- HTML: El esqueleto de una app. Le indica al navegador web cómo desplegar la información<br>- CSS: Indica la apariencia básica, colores, tipos de texto y etiquetas, entre otras cosas<br>-Javascript: A diferencia del HTML, JS permite escoger y tomar decisiones en qué desplegar y en qué momento hacerlo                                                                                                                           |
|  Back-end  | Son las cosas que no se pueden ver de un planeta pero sabes que están ahí<br>- Base de datos: Lugar donde se almacena y recupera la información. La app podría almacenar y recuperar la información acerca de las preferencias de un visitante sobre qué recomendar o no<br>- **Infraestructura**: componentes que dan sustento a la app como el servidor que la aloja, servidores web, almacenamiento, entre otros<br>- **WAF (Web Application Firewall)**: Es opcional sin embargo ayuda a evitar el tráfico peligroso del servidor web |

> **Resumiendo**
> - **Frontend**: Enfocado en la experiencia del usuario
> - **Backend**: Es el cuarto de máquinas que permite a la app web seguir funcionando

___
*Pregunta 1: Which component on a computer is responsible for hosting and delivering content for web applications?* \
**Respuesta: web server**

*Pregunta 2: Which tool is used to access and interact with web applications?* \
**Respuesta: web browser**

*Pregunta 3: Which component acts as a protective layer, filtering incoming traffic to block malicious attacks, and ensuring the security of the the web application?* \
**Respuesta: Web Application Firewall**

### Task 3 — Uniform Resource Locator (URL)
Es la dirección que se pone en el navegador. Permite buscar cualquier tipo de contenido en Internet sin importar si es vídeo, página web, foto o cualquier otro tipo de archivo multimedia.

|        Nombre        | ¿Qué es?                                                                                                         |     Ejemplo      |
| :------------------: | ---------------------------------------------------------------------------------------------------------------- | :--------------: |
|   Esquema (scheme)   | Es el protocolo que se utiliza para entrar a una página web. El más común es `HTTP` y `HTTPS`                    |    `http://`     |
|    Usuario (user)    | Algunas URL pueden incluir detalles de inicio de sesión del usuario para algún sitio que requiera autenticación. | `user:password@` |
|     Host/domain      | Es la parte más importante de una URL ya que indica a qué sitio web se está accesando.                           | `tryhackme.com`  |
|    Puerto (port)     | Ayuda dirigir al navegador directamente al servicio correcto dentro del servidor web                             |      `:80`       |
|     Ruta (path)      | Apunta a un archivo específico o página específica en el servidor al que se está accesando                       |   `/view-room`   |
|     Query string     | Inicia con un signo de interrogación (?). Se utiliza usualmente para buscar términos o campos de formularios     |     `?id=1`      |
| Fragmento (Fragment) | Inicia con un numeral (#) y ayuda a apuntar a una sección específica de una página web                           |     `#task3`     |

> **URL COMPLETA** \
> `http://user:password@tryhackme.com:80/view-room?id=1#task3`

> [!IMPORTANT]
> **Algunas notas aclaratorias**
> - **User**: Es extraño que aparezca en la actualidad ya que poner los detalles de acceso a una cuenta no es muy seguro ya que expone información sensible
> - **Host**: Hay nombres de dominio que tienen pequeñas diferencias con los reales, esta práctica se llama *typosquatting*
> - **Query String**: Los usuarios pueden modificar estas cadenas, es importante saber manejarlas para evitar [inyecciones SQL](../../Logs/Intro%20to%20log%20analysis/TryHackMe%20—%20Intro%20to%20Log%20Analysis.md)
> - **Fragment**: Igual que las anteriores, el usuario las puede modificar. Hay que manejarlas con precaución

***
*Pregunta 1: Which protocol provides encrypted communication to ensure secure data transmission between a web browser and a web server?* \
**Respuesta: HTTPS**

> **RECORDATORIO**: Es la primera parte de la URL

*Pregunta 2: What term describes the practice of registering domain names that are misspelt variations of popular websites to exploit user errors and potentially engage in fraudulent activities?* \
**Respuesta: typosquatting**

> **RECORDATORIO**: Hay dominios que intentan 'suplantar' al sitio web original con alguna variación en la forma de escribir el nombre original

*Pregunta 3: What part of a URL is used to pass additional information, such as search terms or form inputs, to the web server?* \
**Respuesta: Query string**

> **NOTA**: Inicia con el símbolo de interrogación
## Lecciones aprendidas
