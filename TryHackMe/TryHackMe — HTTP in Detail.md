**HyperText Transfer Protocol** (HTTP)
**HyperText Transfer Protocol Secure** (HTTPS)
## Peticiones y respuestas
**Uniform Resource Locator** (URL)
![[Pasted image 20260711150918.png]]
- **Scheme:** con qué protocolo accesar al recurso (HTTP, HTTPS, FTP o SFTP)
- **User:** puedes poner la contraseña y el nombre de usuario ya que algunos sitios necesitan autenticación
- **Host:** nombre de dominio (tryhackme, google, youtube, etc) o dirección IP para accesar al sitio
- **Puerto:** puerto al cual conectarse (`80` usualmente para HTTP y `443` para HTTPS)
- **Query string:** bits extra que se pueden utilizar para pedir otra ruta
- **Fragment:** es una referencia a una locación en en la página actual
### Haciendo una petición
``` http
HTTP/1.1 200 OK

Server: nginx/1.15
Date: Sat, 11 Jul 2026 17:07 GTM
Content-Type: text/html
Content-Length: 98

<html>
<head>
	<title>THM</title>
</head>
<body>
	Bienvenido a TryHackMe!
</body>
</html>
```
### Métodos de `HTML`
`GET` -> Obtiene información del servidor
`POST` -> Envía información al servidor y potencialmente crea nuevos registros
`PUT` -> Actualiza la información dentro del servidor
`DELETE` -> borra información/registros del servidor
## Códigos de estado de `HTTP`

| Código                                  | Respuesta                                                                                                                            |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| 100-199 **Información de la respuesta** | La petición ha sido aceptada por el servidor. Ya no son tan comunes                                                                  |
| 200-299 **Conexión exitosa**            | La petición ha sido exitosa                                                                                                          |
| 300-399 **Redirección**                 | Redirige la petición del cliente a otro recurso. Puede ser desde una página web diferente hasta un sitio web completamente diferente |
| 400-499 **Errores de cliente**          | La petición del cliente ha fallado                                                                                                   |
| 500-599 **Errores de servidor**         | Errores pasando de parte del servidor y usualmente son un tipo de errores de mayor problema                                          |
### Códigos de error comunes

| Código                       | Información                                                                                                                    |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| 200 - OK                     | Petición completada                                                                                                            |
| 201 - Creado                 | Un recurso ha sido creado                                                                                                      |
| 301 - Movido permanentemente | Redirige al navegador del cliente a una nueva página web o le dice al buscador que la página ha sido movida a otro lado        |
| 302 - Encontrado             | Similar al anterior, pero es un cambio temporal y puede cambiar de nuevo                                                       |
| 400 - Bad Request            | Le dice al navegador que algo ha ido mal o se ha perdido en la petición                                                        |
| 401 - Not Authorised         | No estás autorizado a ver este recurso hasta que hayas sido autorizado por la aplicación web (Usuario y contraseña usualmente) |
| 403 - Forbidden              | No tienes permisos para ver los recursos del sitio aunque hayas sido autorizado o no                                           |
| 404 - Page not Found         | La página/recurso no existe                                                                                                    |
| 405 - Method not allowed     | El recurso no permite el método utilizado (usaste un `GET` pero la página solo admite `POST`)                                  |
| 500 - Internal service error | El servidor se ha encontrado con algún tipo de error con la petición y no sabe como manejarla correctamente                    |
| 503 - Service Unavailable    | El servidor no puede manejar la petición. Está sobrecargado o apagado por mantenimiento                                        |
### Headers
- **Host**: le dice al servidor qué recurso estás buscando
- **User-Agent**: El navegador que estás utilizando para darle formato correcto a la petición
- **Content-Length**: Le dice al servidor cuantos datos espera para no perder ninguno al enviar la petición
- **Accept-Encoding**: qué tipos de métodos de compresión admite el navegador
- **Cookies**: Para recordad
- **Content-Type**: Le dice al cliente qué tipo de datos serán devueltos
## Cookies
![[Pasted image 20260711182545.png]]
