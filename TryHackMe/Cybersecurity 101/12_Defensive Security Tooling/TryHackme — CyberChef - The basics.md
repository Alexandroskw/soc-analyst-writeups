# TryHackMe — Nombre de la sala
**Dificultad** -> easy | **Date** -> 16-oct-2026 | **Type** -> Free \
**Sala** -> [CyberChef: The Basics](https://www.tryhackme.com/room/cyberchefbasics)

# Introducción
Sala enfocada en una pequeña introducción en CyberChef, una herramienta tan versátil que se ha convertido en parte importante de los profesionistas en ciberseguridad
# Solución

> [!TIP]
> **CyberChef** puede ser ejecutada de forma local sin necesidad de conexión a internet, descargando la última versión del [repositorio](https://github.com/gchq/CyberChef/releases).

### Task 3 — Navigating the interface
CyberChef se compone en 4 secciones enfocados en diferentes características
1. Operaciones
2. Receta
3. Entrada
4. Salida

![](<./Images/TryHackme — CyberChef - The basics.png>)

| Operaciones   | Descripción                                                                                    | Ejemplos                                                                                                                                                                           |
| ------------- | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Morse to Code | Traduce código morse a caracteres alfanuméricos (en mayúsculas)                                | `- .... .-. . .- - ...` se traduce como `THREATS`                                                                                                                                  |
| URL Encode    | Cifra caracteres problemáticos en cifrado por porcentaje                                       | `https://tryhackme.com/r/room/cyberchefbasics` se vuelve `https%3A%2F%2Ftryhackme%2Ecom%2Fr%2Froom%2Fcyberchefbasics`                                                              |
| To Base64     | Cifra datos sin procesar en cadenas ASCII Base64                                               | `This is fun!` se vuelve `VGhpcyBpcyBmdW4h`                                                                                                                                        |
| To Hex        | Convierte las cadenas entrantes en bytes hexadecimales separados por un delimitador específico | `This Hex conversion is awesome!` se vuelve `54 68 69 73 20 48 65 78 20 63 6f 6e 76 65 72 73 69 6f 6e 20 69 73 20 61 77 65 73 6f 6d 65 21`                                         |
| To Decimal    | Convierte los datos en un arreglo de enteros                                                   | `This Decimal conversion is awesome!` se vuelve `84 104 105 115 32 68 101 99 105 109 97 108 32 99 111 110 118 101 114 115 105 111 110 32 105 115 32 97 119 101 115 111 109 101 33` |
| ROT13         | Cifrado César simple que rota caracteres del alfabeto una cierta cantidad                      | `Digital Forensics and Incident Response` se vuelve `Qvtvgny Sberafvpf naq Vapvqrag Erfcbafr`                                                                                      |

> [!NOTE]
> Las URL y URI soportan la codificación URL Encode \
> El cifrado César por defecto rota 13 lugares

| Área    | Descripción                                                                                                                                                                                                                        | Características                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Recetas | El corazón de la herramienta. Aquí se pueden definir las operaciones que se quieran realizar junto con sus argumentos. Se pueden seleccionar las operaciones que se quieren utilizar específicamente con sus argumentos y opciones | - `Save recipe`: guarda las operaciones seleccionadas por el usuario<br>- `Load recipe`: carga las operaciones guardadas anteriormente<br>- `Clear recipe`: permite limpiar las recetas escogidas durante su uso                                                                                                                                                                                                      |
| Entrada | Es una caja donde se puede introducir tanto texto como otro tipo de archivos                                                                                                                                                       | - `Add a new input tab`: una pestaña adicional es agregada para que el usuario use diferentes valores<br>- `Open folder as input`: Permite al usuario cargar un directorio entero como valor de entrada<br>- `Open file as input`: el usuario puede cargar un archivo<br>- `Clear input and output`: elimina cualquier valor de entrada del usuario<br>- `Reset pane layout`: permite ajustar el ancho de la interfaz |
| Salida  | Es el espacio donde se muestran los datos ya procesados. Presenta de manera ordenada los resultados de cualquier manipulación o transformación que se haya aplicado                                                                | - `Save output to file`: permite a los usuario guardar los resultados en un archivo `.dat`<br>- `Copy raw output to the cipboard`: permite copiar los datos de salida sin procesar directamente<br>- `Replace input with output`: intercambia de lugar la salida con la entrada<br>- `Maximisse output pane`: permite cambiar de tamaño el tamaño de la interfaz                                                      |

***
*Pregunta 1: In which area can you find "From Base64"?* \
**Respuesta: Operations**

*Pregunta 2: Which area is considered the heart of the tool?* \
**Respuesta: Recipe**

### Task 4 — Before Anything Else
Antes de hacer nada en **CyberChef**, es importante resaltar que hay una serie de pasos a seguir
1. Poner un objetivo claro
2. Poner los datos dentro del área de entrada
3. Seleccionar las operaciones que se quieren utilizar
4. Revisar la salida para verificar que es el resultado esperado antes de repetir el proceso 1 o 3

| Paso                                     | Descripción                                                                                                                                                   |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1. Poner objetivos                       | Se deben de proponer metas específicas y alcanzables para responder a la pregunta: **¿Qué quiero completar?**                                                 |
| 2. Poner los datos en el área de entrada | Qué datos se van a utilizar, no se puede hacer nada sin ellos (duh!)<br>                                                                                      |
| 3. Seleccionar las operaciones           | Es un poco engañoso si no se sabe a qué se está enfrentando. Por lo tanto se debe utilizar cualquier operación debajo de la categoría **Encryption/Encoding** |
| 4. Revisar la salida                     | La pregunta implícita: **¿Se alcanzó el objetivo?**. Si la respuesta es si, entonces no hay mas, si no, hay que repetir los pasos                             |
***
*Pregunta 1: At which step would you determine, "What do I want to accomplish?* \
**Respuesta: 1**

> **RECORDATORIO**: Antes de cualquier cosa hay que preguntarnos *¿qué queremos hacer?* o *¿que metas queremos completar?*

### Task 5 — Practice, practice, practice

> Descargar los archivos adjuntos en la tarea

| Extractor               | Descripción                                      |
| ----------------------- | ------------------------------------------------ |
| Extract IP address      | Extrae todas las direcciones IPv/6               |
| Extract URLs            | Extrae las URL de la entrada                     |
| Extract email addresses | Extrae todas las direcciones email de la entrada |

> [!TIP]
> **Algunas aclaraciones**
> - La extracción de direcciones IP extrae cualquier IPv4/6 válidas
> - La extracción de direcciones email obtiene todas las cadenas y caracteres con el formato `johndoe@domain[.]com`
> - Se requiere un protocolo en la extracción de URL (HTTP, FTP, etc) de lo contrario puede generar varios falsos positivos. Para mas detalles revisar [aquí](<../08_Web Hacking/TryHackMe — Web Application Basics.md>)

| Característica       | Descripción                                        |
| -------------------- | -------------------------------------------------- |
| From UNIX timestamp  | Convierte a tiempo UNIX una cadena                 |
| To UNIX to timestamp | Convierte una cadena de tiempo (UTC) a tiempo UNIX |
> [!IMPORTANT]
> El *UNIX timestamp* es un valor de 32-bits que representa el número de segundos transcurridos desde el 1 de enero de 1970, año del nacimiento de UNIX.

| Operaciones | Descripción                                                                                              | Ejemplo                                                                                         |
| ----------- | -------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| From Base64 | Descifra datos de una cadena ASCII Base64 a formato crudo                                                | `V2VsY29tZSB0byB0cnloYWNrbWUh` se vuelve `Welcome to tryhackme!`                                |
| URL Decode  | Convierte los caracteres codificados en formato porcentual de una URI o URL a sus valores sin codificar. | `https%3A%2F%2Fgchq%2Egithub%2Eio%2FCyberChef%2F` se vuelve `https://gchq.github.io/CyberChef/` |
| From Base85 | Codifica arbitrariamente datos de bytes                                                                  | `BOu!rD]j7BEbo7` se vuelve `hello world!`                                                       |
| From Base58 | Codifica arbitrariamente datos de bytes                                                                  | `AXLU7qR` se vuelve `Thm58`                                                                     |
| To Base62   | cifra los datos arbitrariamente los datos de bytes utilizando un juego restringido de símbolos           | `Thm62` se vuelve `6NiRkOY`                                                                     |

> [!IMPORTANT]
> **Acerca de las _Bases_** \
> Las Base(64, 85, 58, 62) se conocen como *Base encodings*, toman datos binarios y lo transforman en una representación basada en texto utilizando código ASCII

> [!NOTE]
> **Sobre Base85 y 62**
> - **Base85** usualmente es más eficiente que **Base64**
> - **Base62** utiliza un juego de símbolos que los humanos convenientemente usan y procesan en las computadoras.

>  Entre más elevada sea la base, la cadena será mas corta que resulte

***
*Pregunta 1: What is the hidden email address?* \
**Respuesta: `hidden@hotmail.com`**

> Utilizar la extracción de email

*Pregunta 2: What is the hidden IP address that ends in .232?* \
**Respuesta: `102.20.11.232`**

> Utilizar la extracción de IP con la casilla de `IPv4` seleccionada (seleccionada por defecto)

*Pregunta 3: Which domain address starts with the letter "T"?* \
**Respuesta: `TryHackMe.com`**

*Pregunta 4: What is the binary value of the decimal number 78?* \
**Respuesta: 01001110**

> **NOTA**: Borrar el archivo cargado anteriormente antes de continuar

*Pregunta 5: What is the URL encoded value of `https://tryhackme.com/r/careers`?* \
**Respuesta: `https%3A%2F%2Ftryhackme%2Ecom%2Fr%2Fcareers`**

> **NOTA**: Marcar la casilla *Encode all special chars*
# Lecciones aprendidas
