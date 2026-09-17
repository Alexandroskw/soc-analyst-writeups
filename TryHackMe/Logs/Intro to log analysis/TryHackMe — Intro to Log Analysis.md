# TryHackMe — Intro to Log Analysis
**Dificultad** -> Fácil | **Date** -> 31-jul-26 | **Type** -> Free
**Sala** -> [Intro to log analysis](https://www.tryhackme.com/room/introtologanalysis)

___
## Introduction
Sala enfocada en la introducción del análisis de los logs, cómo interpretar los datos que generan y cómo identificar las brechas de seguridad.
## Solution
### Task 3 — Investigation theory
Hay varias metodologías, mejores prácticas y técnicas para crea una línea de tiempo coherente para investigaciones efectivas de los logs.
- **Líneas de tiempo**: Es una representación croonológica de los eventos registrados. Una línea de tiempo bien construída permite identificar el punto de origen de un incidente.
- **Timestamps**: Registran cuando un evento ha ocurrido. Algunas herramientas cambian la zona horaria a tiempo UNIX y se almacena en el campo `_time` al ser indexado.
- **Súper líneas de tiempo/línea de tiempo consolidada**: Proveen una línea de tiempo más consolidada de los eventos a través de diferentes fuentes.
- **Visualización de datos**: Permite a los analistas de seguridad entender los datos indexados visualizando patrones y anomalías a través de una interfaz gráfica.
- **Monitoreo de logs y alertas**: Las alertas aseguran que los equipos de seguridad están prontamente notificados cuando hay una actividad inusual o una brecha de seguridad.
- **Investigación externa e Inteligencia de amenazas**: La inteligencia de amenazas son piezas de información que se pueden atribuir a un actor malicioso.

> [!NOTE]
> Splunk puede ayudar en la mayoría de los casos. Sobre todo en **timestamp**,
> la **visualización de datos** y, **monitoreo de logs y alertas**.

___
*Pregunta 1*: *What's the term for a consolidated chronological view of logged events from diverse sources, often used in log analysis and digital forensics?*
**Respuesta: Super timeline**

> **Palabra clave** -> *consolidated*

*Pregunta 2: Which threat intelligence indicator would `5b31f93c09ad1d065c0491b764d04933` and `763f8bdbc98d105a8e82f36157e98bbe` be classified as?*
**Respuesta: File Hashes**
### Task 4 — Detection Engineering
#### Ubicaciones comúnes de los archivos de Log

| Tipo                 | Ubicaciones                                                                                                                                                                                    |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Servidores Web       | **NGINX**:<br>Acceso -> `/var/log/nginx/access.log`<br>Error -> `/var/log/nginx/error.log`<br>**Apache**:<br>Accesso -> `/var/log/apache2/access.log`<br>Error -> `/var/log/apache2/error.log` |
| Base de datos        | **MySQL**:<br>Error -> `/var/log/mysql/error.log`<br>**PostgreSQL**:<br>Error y actividad -> `/var/log/postresql/postgresql-{version}-main.log`                                                |
| Web application      | **PHP**:<br>Error -> `/var/log/php/error.log`                                                                                                                                                  |
| Sistema operativo    | **Linux**<br>General -> `/var/log/syslog`<br>Autenticación -> `/var/log/auth.log`                                                                                                              |
| Firewalls<br>IDS/IPS | **iptables**:<br>Firewalls -> `/var/log/iptables.log`<br>**Snort**:<br>`/var/log/snort/`                                                                                                       |

> [!NOTE]
> Las rutas pueden variar con base en la configuración de los equipos, las versiones de software y configuraciones personalizadas.
#### Patrones comúnes
Los "patrones" son trazas o elementos que los actores maliciosos o amenazas de ciberseguridad dejan atrás en los archivos de logs. Uno de los más comunes es el **comportamiento inusual del usuario**.

> [!INFO]
> **User Behavior Analytics (UBA)**
> Los **UBA** son soluciones enfocadas en determinar patrones de comportamiento normales.
> Ejemplos de ellos son **Splunk UBA** e **IBM QRadar UBA**.


| Tipo de Indicador                     | Significado                                                                                                                                                                                                                                         |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Múltiples intentos fallidos de login  | Un número inusualmente alto de intentos fallidos puede significar un ataque de fuerza bruta.                                                                                                                                                        |
| Tiempos de inicio de sesión inusuales | Logins fuera del horario habitual del usuario puede significar que la cuenta está comprometida.                                                                                                                                                     |
| Anomalías geográficas                 | - Intentos de login desde IP's donde el usuario no es común que inicie sesión puede indicar que la cuenta está comprometida.<br>- Múltiples inicios de sesión de diferentes IP's puede indicar que la cuenta fue compartida o acceso no autorizado. |
| Cadenas inusuales de User-Agent       | Solicitudes de usuarios con cadenas inusuales de User-Agent que se desvían de navegador típico puede indicar un ataque automatizado o actividades maliciosas.                                                                                       |
#### Firmas comúnes de ataque
Contienen características o patrones específicos que quedan atrás y se registran en los archivos de log. Idenetificar estas firmas puede ayudar a responder rápidamente a una amenaza o una rápida detección de la amenaza.

| Nombre de la firma         | Descripción                                                                                                                                         |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| SQL Injection              | Buscar una consulta SQL inusual o malformada en la app o en los logs de la base de datos para identificar patrones comúnes de ataque SQL Injection. |
| Cross-Site Scripting (XSS) | Revisar las entradas en el log con entradas inesperadas o inusuales, usualmente con etiquetas de script como: `<script>`.                           |
| Path Traversal             | Buscar secuencias de caracteres transversales como `../` y `../` e indicadores de acceso a archivos sensibles como `/etc/passwd` y `/etc/shadow`.   |

> [!TIP]
> **Listas de payloads para las firmas comunes de ataque**
> Existen listas útiles para conocer los payloads para Path Traversal y XSS.
> - Para **Path Traversal**, la lista es [esta](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Directory%20Traversal/README.md).
> - Para **XSS**, la lista es [esta](https://github.com/RenwaX23/XSS-Payloads).

___

*Pregunta 1: What is the default file path to view logs regarding HTTP requests on an Nginx server?*
**Respuesta: `/var/log/nginx/access.log`**

*Pregunta 2: A log entry containing `%2E%2E%2F%2E%2E%2Fproc%2Fself%2Fenviron` was identified. What kind of attack might this infer?*
**Respuesta: Path Traversal**

> **NOTA**: Recordar que `%2E` está con codificación URL.

### Task 5 — Automated vs. Manual Analysis
El análisis automatizado involucra herramientas comerciales como XPLG o SolarWinds. Utilizan IA o Machine Learning para identificar patrones.

| Ventajas                                         | Desventajas                                                                |
| ------------------------------------------------ | -------------------------------------------------------------------------- |
| Ahorran tiempo al no ser hecho de forma manual   | Las herramientas son de uso comercial y por lo tanto son caras             |
| La IA ayuda mucho al reconocimiento de patrones. | El reconocimiento de patrones va en función de qué tan capaz es el modelo. |

El análisis manual no depende de herramientas automatizadas, es realizado por personas. El análisis manual es fundamental para un analista ya que no se debde de fiar completamente de las herramientas de automatización.

| Ventajas                                                                                                     | Desventajas                                                                       |
| ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------- |
| Es barato en comparación del análisis automático.                                                            | Requiere mucho tiempo del analista                                                |
| Permite una investigación más profunda                                                                       | -                                                                                 |
| Reduce el riesgo de sobreadaptación o de falsos positivos                                                    | Se pueden pasar por alto ciertas alertas sobre todo si hay un gran número de logs |
| Permite un análisis contextual ya que el analista puede tener un entendimiento más amplio de la organización | -                                                                                 |

___

*Pregunta 1: A log file is processed by a tool which returns an output. What form of analysis is this?*
**Respuesta: Automated**

*Pregunta 2: An analyst opens a log file and searches for events. What form of analysis is this?*
**Respuesta: Manual**
### Task 6 — Command line

> [!NOTE]
> Se puede descargar el archivo adjunto en la tarea o utilizar
> AttackBox dentro de la sala.

La línea de comandos es la forma más rápida que se tiene para hacer un análisis de logs incluso si no se tiene un SIEM configurado.
#### `cat`
puede leer varios archivos y mostrar el contenido en la terminal. Despliega todo el contenido que tenga el (o los archivos)

![](<cat_command.png>)

En el caso de los logs, no es muy útil debido al gran tamaño de estos archivos.

> [!NOTE]
> Existe una variante llamada `bat` que hace exactamente lo mismo pero agrega
> color y número de línea.
> Es la que se esta utilizando en la imagen de arriba.
#### `less`
Es una mejora sobre el comando `cat` para ver archivos de log grandes. Divide el archivo en páginas y se puede desplazar a través de ellas con las flechas o poner el número de la página.

![less](<less_command.png>)
#### `tail`
Está diseñado para ver únicamente el final de los archivos. Útil para ver lo último que se ha ingresado en el contexto de los log. Por defecto, `tail` muestra solamente 10 entradas pero con la bandera `-n` se puede aumentar el número de entradas.

```bash
# Mostrando las últimas entradas por defecto
tail apache.log
```

```bash
# Mostrando las últimas 5 entradas con la bandera -n
$ tail -n 5 apache.log

176.145.201.99 - - [31/Jul/2023:12:34:24 +0000] "GET /login.php HTTP/1.1" 200 1234 "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.90 Safari/537.36"
104.76.29.88 - - [31/Jul/2023:12:34:23 +0000] "GET /index.php HTTP/1.1" 200 5678 "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.81 Safari/537.36"
128.45.76.66 - - [31/Jul/2023:12:34:22 +0000] "GET /contact.php HTTP/1.1" 404 5678 "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.54 Safari/537.36"
76.89.54.221 - - [31/Jul/2023:12:34:21 +0000] "GET /about.php HTTP/1.1" 200 1234 "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.85 Safari/537.36"
145.76.33.201 - - [31/Jul/2023:12:34:20 +0000] "GET /login.php HTTP/1.1" 200 5678 "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.90 Safari/537.36"
```

> [!NOTE]
> El opuesto al comando `tail` es `head` que muestra las primeras entradas del archivo

#### `wc`
Provee el número de líneas, palabras y carácteres en un archivo, lo que puede ayudar a entender qué tan grande es el archivo de logs.

```bash
# Salida por defecto
wc apache-1691435735822.log
70  1562 14305 apache-1691435735822.log

# Número de palabras
wc -w apache-1691435735822.log
1562 apache-1691435735822.log

# Número de líneas
wc -l apache-1691435735822.log
70 apache-1691435735822.log
```
#### `cut`
Es útil cuando en los archivos de logs hay algún separador en específico. Se puede eliminar el separador.

```bash
cut -d ' ' -f 1 apache-1691435735822.log
203.0.113.42
120.54.86.23
185.76.230.45
201.39.104.77
112.76.89.56
211.87.186.35
156.98.34.12
202.176.73.99
...
```

La bandera `-d` señala el delimitador y la bandera `-f` lo despliega en forma de lista.
#### `sort`
Ordena la salida del archivo de log con el criterio que escojamos y de mayor a menor o visceversa.

```bash
cut -d ' ' -f 1 apache-1691435735822.log | sort -n
76.89.54.221
76.89.54.221
76.89.54.221
76.89.54.221
76.89.54.221
76.89.54.221
77.188.103.244
99.76.122.65
...
```

Con la bandera `-n` se ordena la salida de forma numérica. Al agregar la bandera `-r` lo hace de forma descendente. Al agregar la bandera `-n`, la salida se ordena de forma numérica y si se agrega la bandera `-r` lo hace de forma ascendente.

> [!NOTE]
> Para entender mejor estos comandos, se recomienda leer [TryHackMe — Intro to logs](<TryHackMe — Intro to logs.md>)

___

*Pregunta 1: Use `cut` on the `apache.log` file to return only the URLs. What is the flag that is returned in one of the unique entries?*
**Respuesta: c701d43cc5a3acb9b5b04db7f1be94f6**

*Pregunta 2: In the apache.log file, how many total HTTP 200 responses were logged?*
**Respuesta: 52**

> **PRECAUCIÓN**: No hacer caso a la pista que pone la sala. Es mejor utilizar el comando `grep`:
> `grep ' 200 ' apache.log | wc -l`

*Pregunta 3: In the apache.log file, which IP address generated the most traffic?*
**Respuesta: 145.76.33.201**

> Es buena idea utilizar los comandos en conjunto, es decir, primero utilizar `awk` y filtrar la primer columna, luego ordenar, eliminar duplicados, ordenar nuevamente pero de ascendente a descendente:
> `awk '{print $1}' apache.log | sort | uniq -c | sort -nr | head 0`.

*Pregunta 4: What is the complete timestamp of the entry where 110.122.65.76 accessed /login.php?*
**Respuesta: 31/Jul/2023:12:34:40 +0000**

> Se puede utilizar dos veces `grep` utilizando un pipe `|`.

```bash
grep "110.122.65.76" | grep "/login.php"

110.122.65.76 - - [31/Jul/2023:12:34:40 +0000] "GET /login.php HTTP/1.1" 200 9876 "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4638.110 Safari/537.36"
```

### Task 7 — Regular expressions

> [!NOTE]
> Se puede descargar el archivo adjunto en la tarea o utilizar
> AttackBox dentro de la sala.

Las expresiones regulares (o *regex* de forma abreviada) son una forma de buscar patrones, coincidir y manipular datos. Es posible utilizar expresiones regulares junto con el comando `grep`.

```bash
grep -E 'post=1[0-9]' apache-ex2.log
```

Las expresiones regulares son importantes en el parseo de logs, ya que permiten descomponer los archivos de logs en componentes más estructurados y obtener información relevante.
#### **Regular Expressions for Log Parsing**
Se pueden crear patrones personalizados que se adapten a cada tipo de registro. Los patrones sirven para identificar cierta parte de la entrada del log y asignarle un nombre como "date", "user", "IP address", etc., para posteriormente ser buscados en un sistema SIEM de forma mas simple.

> [!TIP]
> Se puede utilizar la herramienta [RegExr](https://regexr.com/) para aprender,
> probar y crear expresiones regulares

____
*Pregunta 1: How would you modify the original grep pattern above to match blog posts with an ID between 20-29?*
**Respuesta: `post=2[0-9]`**

*Pregunta 2: What is the name of the filter plugin used in Logstash to parse unstructured log data?*
**Respuesta: Grok**
### Task 8 — CyberChef

> [!NOTE]
> Se puede descargar el archivo adjunto en la tarea o utilizar
> AttackBox dentro de la sala

Es una herramienta que ofrece más de 300 operaciones que combinadas para hacer una receta, hace que manejar datos sea muy sencillo. Algunas de las herramientas mas importantes son: **codificación y decodificación de datos**, **algoritmos de hasheo y cifrado**, y **análisis de datos**.
En CyberChef también se puede utilizar expresiones regulares.

![](<RegEx en CyberChef.png>)

Para subir un archivo en el campo de "Input" damos clic en este icono

![upload_file](<Subir archivo en CyberChef.png>)

Podemos filtrar por las IP y buscar algún patrón.

![](<Archivo de log subido a CyberChef.png>)

___

*Pregunta 1: Locate the "loganalysis.zip" file under `/root/Rooms/introloganalysis/task8` and extract the contents.*
**No se necesita respuesta**

*Pregunta 2: Upload the log file named "access.log" to CyberChef. Use regex to list all of the IP addresses. What is the full IP address beginning in 212?*
**Respuesta: 212.14.17.145**

*Pregunta 3: Using the same log file from Question #2, a request was made that is encoded in base64. What is the decoded value?*
**Respuesta: THM{CYBERCHEF_WIZARD}**
**Nota**: utilizar rangos en expresiones regulares como `/[A-Za-z0-9+/]{16,}={0,2}`. Se puede hacer uso de RegExr para crear la expresión.

*Pregunta 4: Using CyberChef, decode the file named "encodedflag.txt" and use regex to extract by MAC address. What is the extracted value?*
**Respuesta: 08-2E-9A-4B-7F-61**

> **NOTA**: CyberChef tiene una receta exclusiva para extraer direcciones MAC.

### Task 9 — Yara and Sigma
Sigma es una herramienta Open-Source que puede ser utilizada para crear patrones de búsqueda en un archivo de log. Sigma es utilizado para: **Detectar eventos en logs**, **crear búsquedas en SIEM** e **identificar amenazas**.

```yaml
title: Failed SSH Logins
description: Searches sshd logs for failed SSH login attempts
status: experimental
author: CMNatic
logsource: 
    product: linux
    service: sshd

detection:
    selection:
        type: 'sshd'
        a0|contains: 'Failed'
        a1|contains: 'Illegal'
    condition: selection
falsepositives:
    - Users forgetting or mistyping their credentials
level: medium
```

| Clave          | Valor                     | Descripción                                                          |
| -------------- | ------------------------- | -------------------------------------------------------------------- |
| `a0\|contains` | `a0\|contains: 'Failed'`  | La regla busca las entradas que contengan la palabra clave 'Failed'  |
| `a1\|contains` | `a1\|contains: 'Illegal'` | La regla busca las entradas que contengan la palabra clave 'Illegal' |

Yara es otra herramienta útil para el análisis de logs. Usualmente es más utilizada para la el análisis de malware pero es lo suficientemente versátil para hacer análisis de logs también.

```yaml
rule IPFinder {
    meta:
        author = "CMNatic"
    strings:
        $ip = /([0-9]{1,3}\.){3}[0-9]{1,3}/ wide ascii
 
    condition:
        $ip
}
```


| Clave     | Valor                                            | Descripción                                                        |
| --------- | ------------------------------------------------ | ------------------------------------------------------------------ |
| string    | `$ip = /([0-9]{1,3}\.){3}[0-9]{1,3}/ wide ascii` | Se utiliza regex para crear un patrón de búsqueda para la **IPv4** |
| condition | `$ip`                                            | Si la variable `$ip` se cumple, se dispara la regla                |

___
*Pregunta 1: What languages does Sigma use?*
**Respuesta: YAML**

*Pregunta 2: What keyword is used to denote the "title" of a Sigma rule?*
**Respuesta: title**

*Pregunta 3: What keyword is used to denote the "name" of a rule in YARA?*
**Respuesta: rule**
## Lecciones aprendidas
- Crear una línea de tiempo (sea consolidada o no) es esencial a la hora de empezar el análisis de los archivos de logs.
- Tener ubicadas las rutas de los archivos de logs más comúnes es vital para no perder tiempo valioso a la hora de crear una línea de tiempo.
- Los comportamientos anormales de un usuario son una forma de detectar algún actor malicioso ya que el usuario hace "cosas" que no haría de forma cotidiana o normal.
- Es buena práctica aprender o entender las firmas de ataque más comúnes como XSS o SQL Injection.
- El análisis automático puede eliminar muchas tareas repetitivas pero no sustituye al análisis manual, ya que la experiencia e intuición del analista es muy superior a la hora de detectar un falso o verdadero positivo.
- La línea de comandos es una herramienta prácticamente insustituíble en el arsenal de un analista. Es imperativo que —el analista— aprenda los comandos más básicos y su uso.
- Las expresiones regulares (*regex*) son una de las herramientas de búsqueda de patrones más poderosas que tiene un analista.
- CyberChef es de las herramientas más poderosas que existen para realizar un análisis de archivos de logs.