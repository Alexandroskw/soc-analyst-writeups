# TryHackMe — SOC L1 Alert Triage
**Dificultad** -> easy | **Date** -> 21-oct-2026 | **Type** -> Free \
**Sala** -> [SOC L1 Alert  Triage](https://www.tryhackme.com/room/socl1alerttriage)

## Introducción
Sala enfocada en cómo es que el SOC L1 hace triaje de alertas en un día a día y cómo es que se manejan.
## Solución

> [!NOTE]
> El SOC puede recibir millones de logs al día de miles de sistemas diferentes. Algunos de ellos —los logs— son esperados, otros requieren más atención. \
> Gracias a las alertas, solo se deben de revisar unas docenas a la vez

| Solución     | Ejemplo                               | Descripción                                                                                                                                                                      |
| ------------ | ------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Sistema SIEM | Splunk, Elastic, FortiSIEM            | Filtra las alertas de todos los dispositivos y resalta solo las más relevantes                                                                                                   |
| EDR o NDR    | MS Defender, CrowdStrike, SentinelOne | Se encargan de la detección en los endpoints o en la red. En un SOC se suele utilizar un SIEM o SOAR sobre un EDR o NDR a pesar de que estos últimos proveen su propio dashboard |
| Sistema SOAR | Splunk SOAR, Cortex SOAR              | Centraliza las alertas de otras soluciones de seguridad en un solo lugar y automatiza ciertas tareas                                                                             |
| Sistema ITSM | Jira, TheHive                         | Es el encargado de gestionar los tickets, le asigna una prioridad a cada evento y un especialista para resolverlo                                                                |

### Roles en el triaje

> Los Analistas SOC L1 son los primeros en la línea de defensa y usualmente los que trabajan directamente con las alertas, sin embargo todos en el equipo SOC están involucrados de una u otra manera en el triaje en mayor o menor medida.

| Rol           | Descripción                                                                                                                                        |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| SOC L1        | Revisa las alertas. Hace la distinción entre un falso y un verdadero positivo y escala al L2                                                       |
| SOC L2        | Recibe las alertas escaladas del L1 y hace un análisis más exhaustivo de la alerta                                                                 |
| Ingeniero SOC | El encargado de hacer que las alertas contengan la información suficiente para hacer un buen triaje de las alertas                                 |
| SOC Manager   | Es el encargado del control del SOC. Se asegura que las alertas tengan la calidad suficiente para su clasificación correcta y no se pasen por alto |

### Dashboard de alertas

![SIEM_DASHBOARD](<./Images/THM_SOC.png>)

> El dashboard lista las alertas con el nombre, severidad y estado, aquí se elige de cuál encargarse primero.

### Propiedades de las alertas

| Propiedad                    | Descripción                                                                                                                        | Ejemplo                                                                                                                                  |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Alert Time                   | Fecha y hora de la creación de la alerta. (Por lo regular las alertas se disparan unos minutos después de ocurrido el evento real) | - **Alert time**: Mar 21 2025 @ 13:58<br>- **Event Time**: Mar 21 @ 13:55                                                                |
| Alert Name                   | Breve resumen de lo ocurrido en el evento. Se basa en el nombre de la regla de detección                                           | - Inicio de sesión en una localización desconocida<br>- Phishing<br>- fuerza bruta al RDP<br>- Potencial exfiltración de datos           |
| Alert severity               | Los ingenieros de detección por defecto ponen un nivel de alerta pero los analistas lo pueden cambiar si es necesario              | - 🟢 Bajo o informativo<br>- 🟡 Medio o moderado<br>- 🟠 Alto o severo<br>- 🔴 Crítico o urgente                                         |
| Alert status                 | Da información sobre si alguien ya está trabajando en la alerta o si el triaje ya se ha completado                                 | - New/Unassigned<br>- In Progress/Pending<br>- Closed/Resolved<br>(Hay estados personalizados)                                           |
| Alert verdict/classification | Informa si la alerta es un verdadero o falso positivo                                                                              | - True Positive/Alerta real<br>- False Positive/No es amenaza<br>(Hay veredictos personalizados)                                         |
| Alert Assignee               | Muestra al analista que fue asignado (o si se asignó a sí mismo) a la alerta                                                       | - Usualmente llamado "alert owner"<br>- El asignado toma la responsabilidad de sus alertas                                               |
| Alert Description            | Describe de qué trata la alerta                                                                                                    | - La lógica detrás de la regla que la genera<br>- Por qué esta actividad (puede) indica un ataque<br>- (Opcional) Cómo manejar la alerta |
| Alert fields                 | Da al analista SOC comentarios útiles y evalúa qué disparó la alerta                                                               | - comando introducido a través de línea de comandos<br>- Nombre del host afectado<br>(Otros campos relevantes dependiendo de la alerta)  |

> [!CAUTION]
> El orden de cómo se hace el triaje importa.
> 1. Empezar seleccionando una alerta que no ha sido tomada o resuelta por alguien más.
> 2. Seleccionar las alertas más críticas primero, posteriormente pasar a las altas, luego las medias y así sucesivamente.
> 3. Empezar con las alertas más antiguas y posteriormente pasar a las más nuevas.

### Investigando alertas

![ALERT_ASSIGNATION](<./Images/Asignacion.png>)

> La alerta es crítica. Tiene prioridad, se debe de investigar lo más pronto posible.
> Se debe de empezar asignando a un analista (yo). Se procede a mover el estado a *In Progress* para empezar a familiarizarse con los detalles de la alerta.

> [!TIP]
> Algunos equipos tienen manuales llamados *Workbooks* (o *playbooks*) para ayudar a los analistas L1 a investigar de forma más rápida cierto tipo de alertas.

> [!IMPORTANT]
> De no estar los *playbooks* disponibles se pueden seguir estas recomendaciones
> 1. (Entender) quien (o qué) está bajo ataque (servidor, usuario, sitio web, red, etc).
> 2. Prestar atención a la descripción de la alerta para determinar el tipo de ataque (phishing, malware, inicio de sesión sospechoso).
> 3. Revisar los eventos alrededor del seleccionado. Se pueden encontrar acciones ligadas al incidente poco antes o después de que ocurriera.
> 4. Utilizar herramientas de inteligencia de amenazas o cualquier recurso disponible a la mano, no seguir ciegamente una idea.

> No se debe de seguir una idea ciegamente, es importante tener la información necesaria para determinar si es un verdadero o falso positivo. Por eso el hincapié a utilizar herramientas de inteligencia (por ejemplo: VirusTotal)

### Triaje: tres alertas
#### Alerta 1 — `Potential data exfiltration`

![](<./Images/false_positive.png>)

| Campo          | Contenido                                                                                       |
| -------------- | ----------------------------------------------------------------------------------------------- |
| Severidad      | 🔴 Crítica                                                                                      |
| Host/usuario   | `UK04/MEETINGROOM`                                                                              |
| Qué vi         | Alerta por posible exfiltración de datos sensibles desde un equipo de la organización           |
| Evidencia      | El usuario compartió una reunión grabada a través de la plataforma Zoom. [Destino: `*.zoom.us`] |
| Veredicto      | **Falso positivo**                                                                              |
| Acción         | Se cierra la alerta.                                                                            |
| Siguiente paso | Se sugiere afinar la regla para este tipo de actividades                                        |

#### Alerta 2 — `Double-extension file creation`
En primera instancia, parece un archivo `.mp4` común pero es un ejecutable `*.mp4.exe`.

![](<./Images/video_file.png>)

| Campo           | Contenido                                                                                                     |
| --------------- | ------------------------------------------------------------------------------------------------------------- |
| Severidad       | 🟠 Alta                                                                                                       |
| Host/Usuario    | `LPT-HR-009`                                                                                                  |
| Qué vi          | Archivo con doble extensión. Aparenta ser un vídeo pero es un ejecutable (`.exe`)                             |
| Enriquecimiento | Búsqueda del hash MD5 en [VirusTotal](https://www.virustotal.com). **51/71 motores** lo detectan como malware |
| Veredicto       | **Verdadero Positivo**                                                                                        |
| Acción          | Escalar inmediatamente al L2 en turno y solicitar el aislamiento del host afectado                            |
| Siguiente paso  | Comprobar si el archivo fue ejecutado y comprobar si otros equipos tienen el mismo hash                       |

![](<./Images/double_ext_file.png>)

#### Alerta 3 — `Download from GitHub repository`
Se ha descargado un repositorio desde GitHub. La regla detecta cualquier tipo de descarga de este sitio.

![](<./Images/GitHub_downloads.png>)

| Campo          | Contenido                                                                                                                     |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Severidad      | 🟢 Baja                                                                                                                       |
| Host/usuario   | `LPT-IT-063/G.Chandler`                                                                                                       |
| Qué vi         | Se descargó un repositorio desde GitHub                                                                                       |
| Evidencia      | Se descargó el repositorio de **React**, una biblioteca de JavaScript especializada en el desarrollo de interfaces de usuario |
| Veredicto      | **Falso positivo**                                                                                                            |
| Acción         | Preguntar al miembro del equipo de TI qué es lo que iba a hacer, así mismo verificar si es el repositorio oficial             |
| Siguiente paso | Recomendar al ingeniero SOC que excluya a los miembros de TI y/o alertar por repositorios desconocidos principalmente         |

## Lecciones aprendidas
- Hay que priorizar las alertas con base en su nivel de severidad y antigüedad (primero las críticas y más antiguas).
- Las alertas usualmente se dan pocos minutos después de ocurrido el evento en cuestión.
- Hay varias soluciones de seguridad en el mercado, no solamente un SIEM.
- Los SIEM son (por lo general) la solución más práctica para un SOC.
- El Analista L1 es el primero en la línea de defensa pero todos en un SOC se involucran con el triaje de alertas en mayor o menor medida, no estás solo.
- Es una buena práctica revisar qué descripción da la regla que ha disparado una alerta para saber cómo proceder o identificar un falso de un verdadero positivo.
- Existen una especie de manuales llamados *playbooks* que ayudan a investigar una alerta en particular más eficazmente.
- Un analista L1 no puede hacer más que *recomendar* ajustes al ingeniero SOC sobre las reglas.
- Algo que no conocía del todo era el funcionamiento del **SOAR**, que en esencia es un centralizador de centralizadores. A grandes rasgos, centraliza las demás soluciones de seguridad en un solo lugar, similar a lo que haría un **SIEM** pero a diferencia de este último, puede, por ejemplo bloquear una IP sin intervención manual.
