# TryHackMe — SOC Fundamentals
**Dificultad** -> easy | **Date** -> 15-oct-2026 | **Type** -> Free \
**Sala** -> [SOC Fundamentals](https://www.tryhackme.com/room/socfundamentals)

# Introducción
Sala enfocada en el conocimiento básico acerca del rol de un SOC y qué papel desempeña.

# Solución
### Task 1 — Introduction to SOC

> [!IMPORTANT]
> El **SOC** (**S**ecurity **O**perations **C**enter) está diseñado para administrar la seguridad de una organización, proteger sus activos y monitorear sus redes y sistemas.

***
*Pregunta 1: What does the term SOC stand for?* \
**Respuesta: Security Operations Center**

### Task 2 — Purpose and Components
El equipo SOC tiene a su disposición herramientas que le ayudan a cumplir con el objetivo de mantener a como de lugar intactas las funciones de detección y respuesta, conocidas como Soluciones de Seguridad (**Security Solutions**)

> [!NOTE]
> Para concretar el objetivo de mantener la detección y la respuesta intactos, es necesario que el equipo SOC esté monitoreando constantemente la red y los dispositivos en ella.

| Detección                                                                                                                                            | Respuesta                                   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| - **Detectar vulnerabilidades**<br>- **Detectar actividad no autorizada**<br>- **Detectar infracciones de las políticas**<br>- **Detectar intrusos** | - **Soporte con la respuesta a incidentes** |

> [!TIP]
> **Los pilares de un SOC**
> - Gente
> - Proceso
> - Tecnología

***
*Pregunta 1: The SOC team discovers an unauthorized user is trying to log in to an account. Which capability of SOC is this?* \
**Respuesta: Detection**

> **NOTA**: En qué categoría cae (Respuesta o Detección) 

*Pregunta 2: What are the three pillars of a SOC?* \
**Respuesta: People, Process, Technology**

### Task 3 — People

> **Siempre** será necesario que haya personas en un SOC para filtrar el ruido de las alertas y detectar verdadera actividad maliciosa


| Rol                | Actividad                                                                                                                                                                                             |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SOC L1             | Es el primero en responder a cualquier alerta. Realiza una clasificación básica para determinar si una detección es peligrosa o                                                                       |
| SOC L2             | Va más allá del SOC L1. Hace investigaciones más profundas que el L1 y correlaciona los datos de múltiples fuentes para realizar un análisis más detall                                               |
| SOC L3             | Son más experimentados. Buscan más en detalle sobre indicadores de amenazas y apoyan a las actividades de respuesta a incidentes incluyendo la contención, erradicación y recuperación                |
| Security Engineer  | Despliegan y configuran las soluciones de seguridad para que todo funcione correcta                                                                                                                   |
| Detection Engineer | Los L2 y L3 usualmente crean reglas para las soluciones de seguridad, algunas veces el SOC puede recurrir a este puesto independientemente para crear las reglas                                      |
| SOC Manager        | Administra los procesos que sigue el SOC y da apoyo. Además mantiene contacto con el CISO de la organización para dar actualizaciones durante la postura actual de seguridad y los esfuerzos actuales |
***
*Pregunta 1: Alert triage and reporting is the responsibility of?* \
**Respuesta: SOC Analyst (Level 1)**

*Pregunta 2: Which role in the SOC team allows you to work dedicatedly on establishing rules for alerting security solutions?* \
**Respuesta: Detection Engineer**

### Task 4 — Process

> [!IMPORTANT]
> **Acerca de las *Alert Triage*** \
> La primer medida a cualquier alerta es clasificarla. Determina la severidad de la misma y con base a ello, darle una prioridad.

|     Las 5 W      | Respuestas                                                                                                                                   |
| :--------------: | -------------------------------------------------------------------------------------------------------------------------------------------- |
|  What? (¿Qué?)   | Un archivo malicioso ha sido detectado en la red interna de la organización                                                                  |
| When? (¿Cuándo?) | El archivo fue detectado a cierta hora                                                                                                       |
| Where? (¿Dónde?) | El archivo fue detectado en el directorio de un equipo en concreto                                                                           |
|  Who? (¿Quién?)  | El archivo fue detectado por el usuario del equipo                                                                                           |
| Why? (¿Por qué?) | Después de la investigación, el archivo fue descargado de un sitio web de programas piratas. El usuario quería el software de forma gratuita |

> [!IMPORTANT]
> **Los reportes** \
> Las alertas de amenaza deben ser enviadas a un analista de nivel superior para su pronta y solución oportunas. El informe debe abordar las 5 preguntas. Debe tener un análisis exhaustivo y capturas de pantalla como evidencia.

> Algunas detecciones reportadas pueden significar actividades de alto riesgo, cuando esto ocurre los equipos de alto nivel deben realizar una respuesta a incidentes (Incident Response)

> [!TIP]
> En ocasiones una actividad forense detallada puede ser requerida debido a que apunta a determinar la raíz del incidente.

***
*Pregunta 1: At the end of the investigation, the SOC team found that John had attempted to steal the system's data. Which 'W' from the 5 Ws does this answer?* \
**Respuesta: Who**

> **RECORDATORIO**: Se está cuestionando *quién* lo hizo

*Pregunta 2: The SOC team detected a large amount of data exfiltration. Which 'W' from the 5 Ws does this answer?* \
**Respuesta: What**

> **RECORDATORIO**: ¿*Qué* se se está escapando?

### Task 5 — Technology

> El pilar de **Tecnología** en un SOC son las Soluciones de Seguridad (Security Solutions)

> [!NOTE]
> Las Soluciones de seguridad centralizan toda la información de los dispositivos o aplicaciones dentro de la red de la organización y automatizan la detección y respuesta

- **SIEM (Security Information and Event Management)**: Colecta los logs de varios dispositivos en la red. Aquí es donde se crean las reglas de detección encargadas de identificar la actividad sospechosa
- **EDR (Endpoint Detection and Response)**: Da detalles en tiempo real del historial del dispositivo en el que se instaló y hace las alertas automáticamente
- **Firewall**: Funciona enteramente para la seguridad de la red. Es una barrera entre la red interna y el Internet (principalmente). Filtra el tráfico entrante no autorizado

> [!NOTE]
> Hay más soluciones que desempeñan un rol único en un SOC como EPP, IDS/IPS, XDR, SOAR entre otros. Todo depende de qué se quiere realizar y los recursos de la organización

***
*Pregunta 1: Which security solution monitors the incoming and outgoing traffic of the network?* \
**Respuesta: Firewall**

*Pregunta 2: Do SIEM solutions primarily focus on detecting and alerting about security incidents? (yea/nay)* \
**Respuesta: yea**

> **NOTA**: Al centralizar todos los logs de los endpoints, es más sencillo detectar y alertar sobre incidentes de seguridad

### Task 6 — Practical Exercise
***
*Pregunta 1: What: Activity that triggered the alert?* \
**Respuesta: Port scan**

*Pregunta 2: When: Time of the activity?* \
**Respuesta: June 12, 2024 17:24**

> **RECORDATORIO**: El timestamp es por si solo indicador de actividad sospechosa

*Pregunta 3: Where: Destination host IP?* \
**Respuesta: `10.0.0.3`**

*Pregunta 4: Who: Source host name?* \
**Respuesta: NESSUS**

*Pregunta 5: Why: Reason for the activity? Intended/Malicious* \
**Respuesta: Intended**

> **NOTA**: Está dentro del mismo rango de IP

*Pregunta 6: Additional Investigation Notes: Has any response been sent back to the port scanner IP? (yea/nay)* \
**Respuesta: yea**

*Pregunta 7: What is the flag found after closing the alert?* \
**Respuesta: THM{000_INTRO_TO_SOC}**

# Lecciones aprendidas
- Los SOC son la primera línea de defensa ante un ataque en una organización
- Las bases fundamentales de un SOC son su gente, el proceso y las tecnologías que utiliza
- La principal tarea de un SOC es mantener la capacidad de detección y respuesta ante un ataque
- Si el SOC es la primera línea de defensa, el Analista SOC L1, sería el explorador o vigía, que alerta al resto de las tropas sobre un posible ataque enemigo
- El Analista SOC L2 es el encargado de hacer una investigación mas profunda con base a las alertas que le haga llega el Analista L1, serían una especie de reconocimiento aéreo
- El Analista SOC L3 está activamente buscando indicadores de amenazas. Son como los hospitales de campaña en la retaguardia. Se encargan de hacer contención, erradicación y recuperación después de una respuesta a incidentes
- El Security Engineer es el encargado de desplegar, instalar y configurar cada tecnología que el SOC utilice. Sería el artillero
- Los Detection Engineer son, hasta cierto punto, prescindibles ya que tanto el L2 como el L3 puede configurar también las soluciones de seguridad. Todo va a depender de cada organización
- El SOC Manager es el líder del equipo SOC, se encarga de dar apoyo y administrar los procesos en el SOC, es el Mariscal de Campo
- Antes de nada, se debe de clasificar una alerta con base en qué tan grave es y si es prioritaria o no
- Con base en el punto anterior se pueden hacer 5 preguntas: ¿Qué?, ¿Cuándo?, ¿Dónde?, ¿Quién? y ¿Por qué?
- Las alertas en forma de reportes, se llaman tickets y pasan a analistas de nivel superior para su análisis
- Las tecnologías son lo que a un soldado son sus armas. Hacen la vida más sencilla para que el analista no haga todo de forma manual
- Con base a lo anterior, se debe de determinar qué tipo de tecnologías se van a utilizar después de haber hecho una evaluación de las amenazas y recursos disponibles en la organización