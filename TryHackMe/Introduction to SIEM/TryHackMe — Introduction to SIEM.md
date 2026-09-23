# TryHackMe — Introduction to SIEM
**Dificultad** -> easy | **Date** -> 27-jul-26 | **Type** -> Free \
**Sala** -> [Intro to SIEM](https://www.tryhackme.com/room/introtosiem)

## Introduction
La sala se enfoca en el **Security Information and Event Management system** (**SIEM**) que es la solución de seguridad principal que tiene un Analista SOC, qué hace, como ingiere los logs y cómo funciona el flujo de una alerta.

## Solution

### Task 1 — Introduction
*Pregunta 1: What does SIEM stand for?* \
**Respuesta: Security Information and Event Management system**.

### Task 2 — Logs everywhere, answers nowhere
#### Logs everywhere

A los dispositivos los podemos llamar fuentes de registros (log sources). Se utilizan para identificar actividad maliciosa o soluciones a problemas. Se dividen en dos tipos:
- **Host-Centric log sources**.
- **Network-centric log sources**.

#### Host-Centric Log Sources

Los dispositivos que generan este tipo de logs son windows, linux, servidores, etc., básicamente endpoints:
- Un usuario accesando a un archivo o intentando autenticarse
- Un proceso en ejecución realiza una actividad
- Un proceso agregando/editando/borrando una llave de registro
- Ejecución de comandos de PowerShell

#### Network-Centric Log Sources
Son los registros que se generan cuando los dispositivos se comunican entre ellos o cuando acceden a intenet. Los dispositivos que generan estos registros son IPS/IDS, firewall, routers, etc. Algunos ejemplos son:
- SSH
- Un archivo accesado vía FTP
- Tráfico de red
- Un usuario accediendo a un recurso de la empresa por medio de VPN
- Archivos de red compartiendo actividad

Crean una gran cantidad de registros constantemente

#### Answers Nowhere
Hay algunos desafíos a los que nos enfrentamos al analizar los logs:
- **Numerosas fuentes de logs**: Las redes tienen muchas fuentes que generan logs y a su vez generan cientos de eventos por segundo.
- **No centralización**: Los logs residen en el dispositivo que los generó y para analizarlos se necesitaría conectarse vía **SSH** o **RDP** para analizarlos.
- **Contexto limitado**: Si los logs de diferentes fuentes de están relacionados, pueden decir una historia diferente a la que están contando individualmente.
- **Análisis limitado:** Las fuentes de logs generan cientos de logs por segundo y revisar cada dispositivo es imposible para un humano.
- **Errores de formato**: Las fuentes de logs los generan de varias formas.

---
*Pregunta 1: Is Registry-related activity host-centric or network-centric?* \
**Respuesta: host-centric**

> Los registros se relacionan directamente con los host.

*Pregunta 2: Is VPN-related activity host-centric or network-centric?* \
**Respuesta: newwork-centric**

> Las VPN se relacionan directamente con Internet.

### Task 3 — Why SIEM?
Un **SIEM** es la mejor manera de **centralizar** todos los logs que se generan en los dispositivos de una red. Toma los logs de diferentes fuentes, estandariza su formato, los correlaciona y detecta actividades maliciosas con reglas de detección.

> **Palabra clave** -> **_CENTRALIZAR_**.

Algunos de los paneles de control por defecto de la mayoría de los SIEM son:
- Alertas destacatadas.
- Notificaciones del sistema.
- Alerta de salud.
- Lista de inicios de sesión fallidos.
- Cuenta de los eventos ingeridos.
- Reglas activadas.
- Principales dominios visitados.

### Task 4 — Log sources and Ingestion
#### Windows machine
Windows registra cada evento que pueda ser visto por el **Event Viewer**. Le asigna un ID único a cada actividad para que el analista pueda examinar el evento y seguirlo.

![Event_viewer_interface](<./Images/Event Viewer en Windows.png>)

#### Linux machine
Algunas de las locaciones más comunes donde Linux almacena los logs son:
- `/var/log/httpd` o `/var/log/apache`: Logs de petición o respuesta y de errores de HTTP.
- `/var/log/cron`: Eventos relacionados con trabajos cron.
- `/var/log/auth.log` o `/var/log/secure`: Logs relacionados con autenticación. Los primeros son para sistemas basados en Debian y los últimos para sistemas basados en REHL.
- `/var/log/kern`: Logs relacionados con eventos del kernel.

> [!TIP]
> **Herramienta útil para visualizar logs — `lnav`**
> 
> Es un visualizador CLI para ver los logs de forma ordenada.
> Referencia: [Linux Log Files Finally Make Sense with lnav](https://youtu.be/z0jBa_mkui0?si=6dfF6No5I6JwVQ7x)

---

*Pregunta 1: In which location within a Linux environment are HTTP logs stored?* \
**Respuesta: /var/log/httpd**

> Los logs relacionados al protocolo HTTP(S) se almacenan en el registro del mismo nombre.

### Task 5 — Alerting processing and analysis
#### Behind the triggered alerts
Los SIEM tienen reglas de detección diseñadas para detectar amenazas:
- Si un usuario ha fallado 5 veces en el inicio de sesión en 10 segundos levantar una alerta que diga **Multiple failed login attempts**.
- Si el inicio de sesión es exitoso después de múltiples fallas levantar una alerta que diga **Successful login after multiple login attempts**.
- Una regla para que se lance una alerta cada que un usuario conecta un dispositivo USB (útil si los USB están restringidos por políticas de la compañía).
- Si el tráfico de salida es >25MB lanzar una alerta para un posible intento de exfiltración de datos.

| EventID  | Indicador                                                                               |
| -------- | --------------------------------------------------------------------------------------- |
| **104**  | Se ha limpiado la tabla de Event Log. Alguien quiere borrar su rastro.                  |
| **4688** | Se ha ejecutado un nuevo proceso. Alguien ha ejecutado un comando `whoami`, `net user`. |

---

*Pregunta 1: Which Event ID is generated when event logs are removed?* \
**Respuesta: 104**

> El ID se crea cada que ha habido un cambio en el event log.

*Pregunta 2: What type of alert may require tuning?* \
**Respuesta: False positive**

> Las alertas que resultan en un **Falso Positivo** necesitan ser ajustadas para que no vuelva a ocurrir en el futuro.

### Task 6 — Lab Work
**Inicio del laboratorio**: Start Suspicious activity

![Dashboard_example](<./Images/dashboard.png>)

**Alerta configurada del SIEM**: una potencial actividad de criptominería observada.

![](<./Images/Alert.png>)

**Ubicar al usuario**: Se debe ubicar al usuario responsable que disparó la alerta por medio de la tabla de eventos.

![](<./Images/events.png>)

> [!NOTE]
> Se puede identificar al usuario debido a la columna **ProcessName**. \
> Ya que está ligada directamente al nombre de usuario

![](<./Images/user.png>)

Nombre del host (hostname) del equipo infectado obtenido al mismo tiempo que el nombre del usuario: **HR_02**. \
Causa de la alerta disparada: la alerta se dispara si contiene la palabra *miner* o *crypt*.

**Causa de la alerta disparada**: la alerta se dispara si contiene la palabra _miner_ o _crypt_. **miner** en el caso actual.

![](<./Images/rule.png>)

Determinar Falso o Verdadero Positivo: Verdadero Positivo; el host necesita ser aislado. 

---
*Pregunta 1: After clicking on the **Start Suspicious Activity button**, which process caused the alert?* \
**Respuesta: `cudominer.exe`**

> El nombre del archivo aparece parpadeando en rojo al inicio del laboratorio.

*Pregunta 2: Find the event that caused the alert and identify the user responsible for the process execution* \
**Respuesta: chris**

*Pregunta 3: What is the hostname of the suspect user?* \
**Respuesta: HR_02**

*Pregunta 4: Examine the rule and the suspicious process; which term matched the rule that caused the alert?* \
**Respuesta: miner**

*Pregunta 5: Which option best represents the event? Choose from the following: False Positive or True Positive* \
**Respuesta: True Positive**

*Pregunta 6: Selecting the right ACTION will display the FLAG. What is the FLAG?* \
**Respuesta: THM{000_SIEM_INTRO}**

## Lecciones aprendidas
- Los EventID `104` y `4688` son vitales en las detecciones de equipos Windows. Se deben memorizar o cuando menos tener presente en todo momento.
- El SIEM es una herramienta, el analista lo debe configurar. Los falsos positivos también son parte del día a día.
- El flujo de triaje siempre es: **alerta → contexto → decisión → acción**.

# Referencias
[TryHackMe — Network Traffic Basics](<../Network/Network Traffic Basics/TryHackMe — Network Traffic Basics.md>)