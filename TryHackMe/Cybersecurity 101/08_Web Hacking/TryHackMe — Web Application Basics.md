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

> [!TIP]
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

### Task 4 — HTTPS Messages

> Son paquetes que se intercambian entre el usuario y el servidor.

> [!NOTE]
> Los mensajes ayudan a entender las peticiones del cliente y las respuestas del servidor con el que se está comunicando. \

> [!IMPORTANT]
> **¿Cómo nos vamos a comunicar?** \
> Existen dos tipos de mensajes
> - **HTTP response**: Es enviada por el usuario para lanzar ciertas acciones
> - **HTTP request**: Es enviada por el servidor en respuesta a la petición del cliente

|   Campo    | ¿Qué es?                                                                                                                        |                        Ejemplo                         |
| :--------: | ------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------: |
| Start line | Es la primera línea del mensaje, por lo tanto indica si es una petición del usuario o una respuesta del servidor                |                 `POST /login HTTP/1.1`                 |
|  Headers   | Están compuestos por pares clave-valor que proveen información adicional acerca del mensaje                                     | `Content-Type: application/json<br>Content-Length: 56` |
| Empty line | Es un divisor. Separa el header del cuerpo del mensaje                                                                          |                   `<SALTO_DE_LINEA>`                   |
|    Body    | Es donde se almacenan los datos del mensaje. El cuerpo puede incluir datos que el usuario quiere enviar al servidor como datos. |      `username=alejandro&password=1234seguridad`       |

> [!IMPORTANT]
> **La comunicación es la clave de una relación** \
> Entender los HTTP Messages es importante
> - Son la base fundacional de cómo es que se comunica una app web
> - Entender cómo funcionan puede ayudar a entender y diagnosticar problemas de comunicación web
> - Es crucial para la seguridad. Entender los mensajes puede ayudar a crear medidas de seguridad más estrictas para proteger los datos durante la transmisión

***
*Pregunta 1: Which HTTP message is returned by the web server after processing a client's request?* \
**Respuesta: HTTP Response**

> **PALABRAS CLAVE** -> *returned by the web server*

*Pregunta 2: What follows the headers in an HTTP message?* \
**Respuesta: Empty Line**


### Task 5 — HTTP Request: Request line and Methods
Las **HTTP Request** es lo que un usuario envía a un servidor web esperando a que algo suceda en una aplicación web. Estas peticiones son (usualmente) el primer punto de contacto entre el usuario y el servidor. Es imperativo entender cómo funcionan.

| Línea        | Qué es                                                                                                                 | Ejemplo                     |
| ------------ | ---------------------------------------------------------------------------------------------------------------------- | --------------------------- |
| Request line | Le dice al servidor qué se está solicitando. Se divide en tres partes: **HTTP Method**, **URL path** y **URL Version** | `METHOD /path HTTP/version` |
| URL Path     | Le dice al servidor donde puede encontrar el recurso solicitado.                                                       | `GET /api/user`             |
| HTTP Version | Muestra la versión del protocolo que se está utilizando entre el cliente y el servidor                                 | `GET /api/user HTTP/1.1`    |

| Métodos   | Descripción                                                                                            |
| --------- | ------------------------------------------------------------------------------------------------------ |
| `GET`     | Obtiene los datos del servidor sin modificarlos                                                        |
| `POST`    | Envía datos al servidor                                                                                |
| `PUT`     | Reemplaza o actualiza información en el servidor                                                       |
| `DELETE`  | Remueve información del servidor                                                                       |
| `PATCH`   | Actualiza la parte de un recurso. Utilizado para realizar pequeños cambios en lugar de reemplazar todo |
| `HEAD`    | Funciona como `GET` pero solo obtiene las cabeceras, no todo el contenido                              |
| `OPTIONS` | Despliega los métodos que están disponibles para un recurso específico                                 |
| `TRACE`   | Similar a `OPTIONS` pero usualmente para hacer debugging                                               |
| `CONNECT` | Crea una conexión segura como `HTTPS`                                                                  |

> [!NOTE]
> Los 4 primeros métodos son los más populares pero es buena práctica entender que hay más y que se utilizan en contextos diferentes

> [!IMPORTANT]
> Los protocolos `HTTP/2` y `HTTP/3` son más modernos pero en la actualidad, la versión `HTTP/1.1` es la más versátil y utilizada. Muchos sistemas la siguen utilizando

***
*Pregunta 1: Which HTTP protocol version became widely adopted and remains the most commonly used version for web communication, known for introducing features like persistent connections and chunked transfer encoding?* \
**Respuesta: `HTTP/1.1`**

*Pregunta 2: Which HTTP request method describes the communication options for the target resource, allowing clients to determine which HTTP methods are supported by the web server?* \
**Respuesta: `OPTIONS`**

> **NOTA**: El método dice qué tipo de peticiones están disponibles para ese recurso en específico

*Pregunta 3: In an HTTP request, which component specifies the specific resource or endpoint on the web server that the client is requesting, typically appearing after the domain name in the URL?* \
**Respuesta: URL Path**

> **RECORDATORIO**: El **URL Path** indica en dónde se encuentra el recurso solicitado

### Task 6 — HTTP Request: Headers and Body

| Header       | Descripción                                                                                                     | Ejemplo                                                                          |
| ------------ | --------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Host         | Especifica el nombre del servidor web que se está solicitando                                                   | `Host: tryhackme.com`                                                            |
| User-Agent   | Comparte la información del navegador que hace la solicitud                                                     | `User-Agent: Mozilla/5.0`                                                        |
| Referer      | Indica la URL de donde proviene la solicitud.                                                                   | `Referer: https://www.google.com`                                                |
| Cookie       | La información que el servidor web le pidió anteriormente al navegador que almacenara se guarda en las cookies. | `Cookie: user_type=student; room=introtowebapplication; room_status=in_progress` |
| Content-Type | Describe qué tipo o formato de datos están en la solicitud                                                      | `Content-Type: application/json`                                                 |
#### Request Body
Algunas **HTTP Request** como `PUT` y `POST` los datos están alojados dentro del body de la solicitud. El formato puede ser de varias formas pero los más populares son: `URL Encoded`, `Form Data`, `JSON` y `XML`.
- **URL Encoded (application/x-www-form-urlencoded**: Los datos están estructurados en pares de clave y valor (`key=value`). Si hay muchos pares, se deben separar por el símbolo `&`. Los caracteres especiales están codificados con `%`
  
```http
POST /profile HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 33

name=Aleksandra&age=27&country=US
```

- **Form data (multipart/form-data)**: Permite múltiples bloques de datos para ser enviados. Cada bloque está separado por una cadena delimitadora. Se utiliza para enviar información binaria (por ejemplo, subir imágenes a un servidor web)

```http
POST /upload HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="username"

aleksandra
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="profile_pic"; filename="aleksandra.jpg"
Content-Type: image/jpeg

[Binary Data Here representing the image]
----WebKitFormBoundary7MA4YWxkTrZu0gW--
```

- **JSON (application/json)**: Los datos son enviados en JSON (JavaScript Object Notation). Se separan en pares de `"name":"value"` (múltiples valores se separan por comas (,))
  
```http
POST /api/user HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/json
Content-Length: 62

{
    "name": "Aleksandra",
    "age": 27,
    "country": "US"
}
```

- **XML (application/xml)**: Los datos están estructurados dentro de etiquetas llamadas **tags**, una de apertura y otra de cierre. Pueden ser anidadas una dentro de otra
  
```http
 POST /api/user HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/xml
Content-Length: 124

<user>
    <name>Aleksandra</name>
    <age>27</age>
    <country>US</country>
</user>
```

***
*Pregunta 1: Which HTTP request header specifies the domain name of the web server to which the request is being sent?* \
**Respuesta: Host**

> **RECORDATORIO**: Se está *pidiendo* el nombre del servidor al que enviamos la solicitud

*Pregunta 2: What is the default content type for form submissions in an HTTP request where the data is encoded as key=value pairs in a query string format?* \
**Respuesta: `application/x-www-form-urlencoded`**

> **PALABRAS CLAVE** -> `key=value`

*Pregunta 3: Which part of an HTTP request contains additional information like host, user agent, and content type, guiding how the web server should process the request?* \
**Respuesta: Request Headers**

> **RECORDATORIO**: Los headers son los que tienen la información de cómo es que el servidor y el cliente se van a comunicar

### Task 7 — HTTP Response: Status Line and Status Code
Al interactuar con una app web, el servidor envía una **HTTP Response** de vuelta para confirmar si la solicitud fue exitosa o pasó algo más. Esta respuesta viene acompañada de un **status code** (código de estado) y una explicación breve que da una pista de cómo es que el servidor manejó la petición

> La explicación breve es conocida como **Reason Phrase**

> [!IMPORTANT]
> La primera línea de cada HTTP Response se le conoce como **Status Line** que proporciona la siguiente información
> - **HTTP Version**: Versión del protocolo HTTP utilizada
> - **Status Code**: Número de tres dígitos que muestra el resultado de la solicitud
> - **Reason Phrase**: Breve mensaje explicando el código de estado en términos humanos

| Nombre                  | Descripción                                                                        | Código  |
| ----------------------- | ---------------------------------------------------------------------------------- | :-----: |
| Informational responses | El servidor recibió parte de la solicitud y espera el resto                        | 100-199 |
| Succesful Responses     | Todo ha salido bien. Se ha establecido conexión y el servidor respondió como debía | 200-299 |
| Redirection Messages    | El recurso solicitado ha sido movido a una locación diferente                      | 300-399 |
| Client Error Responses  | Indican un problema con la solicitud. Tal vez una URL mal dada                     | 400-499 |
| Server Error Responses  | El servidor encontró un error mientras procesaba la solicitud                      | 500-599 |

> [!TIP]
> Algunos de los códigos más comunes son
> - **100 (Continue)**: El servidor obtuvo la primera parte de la solicitud. Se prepara para el resto
> - **200 (OK)**: La solicitud fue exitosa. Se ha enviado el recurso solicitado
> - **301 (Moved permanently)**: El recurso solicitado ha sido movido permanentemente a una nueva URL
> - **404 (Not Found)**: El servidor no pudo encontrar el recurso a través de la URL dada por el usuario
> - **500 (Internal Server Error)**: Algo ha ido mal con el servidor

***
*Pregunta 1: What part of an HTTP response provides the HTTP version, status code, and a brief explanation of the response's outcome?* \
**Respuesta: Status Line**

> **RECORDATORIO**: La línea de estado es lo primero que aparece en la HTTP Response

*Pregunta 2: Which category of HTTP response codes indicates that the web server encountered an internal issue or is unable to fulfil the client's request?* \
**Respuesta: Server Error Responses**

> **PALABRAS CLAVE** -> *server encountered an internal issue*

*Pregunta 3: Which HTTP status code indicates that the requested resource could not be found on the web server?* \
**Respuesta: 404**

### Task 8 — HTTP Response: Headers and Body

> [!TIP]
> Los headers de **HTTP Response** dan información importante acerca de la respuesta y le dice al cliente que usualmente es un navegador, cómo manejarla

| Response Header | Descripción                                                                                                    | Ejemplo                                  |
| --------------- | -------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| Date            | Muestra la fecha y hora exactas cuando se generó la solicitud en el servidor                                   | `Date: Fri, 23 Aug 2024 10:43:21 GTM`    |
| Content-Type    | Qué contenido se está obteniendo, por ejemplo JSON, o HTML (también incluye un juego de caracteres como UTF-8) | `Content-Type: text/html; charset=utf-8` |
| Server          | Indica qué software para servidor se está utilizando                                                           | `Server: nginx`                          |
| Set-Cookie      | Envía las cookies del servidor al cliente y este último las almacena para enviarlas de vuelta en el futuro     | `Set-Cookie: sessionId=38af1337es7a8`    |
| Cache-Control   | Le dice al cliente qué tan larga puede ser la caché de la respuesta antes de volver a revisar el servidor      | `Cache-Control: max-age=600`             |
| Location        | Se utiliza                                                                                                     |                                          |

## Lecciones aprendidas
