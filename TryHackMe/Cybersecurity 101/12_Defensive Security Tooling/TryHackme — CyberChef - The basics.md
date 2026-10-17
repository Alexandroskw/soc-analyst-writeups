# TryHackMe — Nombre de la sala
**Dificultad** -> easy/medium/hard | **Date** -> 16-oct-2026 | **Type** -> Free/Premium \
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

# Lecciones aprendidas
