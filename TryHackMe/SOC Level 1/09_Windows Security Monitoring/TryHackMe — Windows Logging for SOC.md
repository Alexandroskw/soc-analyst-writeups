# TryHackMe — Windows Logging for SOC
**Dificultad** -> easy | **Date** -> 26-nov-2026 | **Type** -> Free \
**Sala** -> [Windows Logging for SOC](https://www.tryhackme.com/room/windowsloggingforsoc)

## Introducción
Sala enfocada en el monitoreo de sistemas Windows y entender cómo utilizar sus sistemas de logs para detectar amenazas.

## Solución
### What is Logged
Cada acción que se realice dentro del Sistema Operativo como abrir un programa, crear un archivo o solo iniciar sesión. El SO registra el evento y lo adjunta en el diario (journal), iniciando con la hora, qué acción se realizó y qué usuario la realizó.
Un registro adecuado ayuda a un SOC a:
- **Incident Response**: los logs pueden mostrar cuándo y cómo ocurrió un ataque
- **Threat hunting**: Permiten buscar señales de actividad maliciosa
- **Alerting and Triage**: Son un elemento fundamental de cualquier alerta o detección

#### La anatomía de una entrada de Log

> [!TIP]
> Los archivos de log dentro de Windows se almacenan en:
> `C:\Windows\System32\winevt\Logs` \
> Y la extensión es `.evtx`

Cada archivo de log corresponde a una categoría en específico, por ejemplo los **Application Logs** registran eventos de aplicaciones en modo de usuario, los **Security Logs** capturan eventos como intentos de inicio de sesión, actividad del proceso y administración de usuarios.

#### Cómo leer los eventos
Dentro de Windows existe la herramienta llamada **Event Viewer** y se divide en 4 partes fundamentales:
- **Log Source**: Cada archivo `evtx` corresponde a un elemento
- **Log List**: Cada fila es un único evento que contiene unas cuantas propiedades:
	- **Keywords**: Indica si la acción fue exitosa o no (para algunos eventos)
	- **Date and time**: La marca de tiempo de cuando ocurrió el evento
	- **Event ID**: Un número único fara el nombre del evento
- **Log Details**: El contenido actual del registro (puede ser en texto plano o en XML)
- **Filters Menu**: filtra los registros

### Security Log: Authentication

#### Dos de los más importantes Security Logs

| Event ID                    | Propósito                                                                                                    | Limitaciones                                                                               |
| --------------------------- | ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------ |
| **4624** (Successful Logon) | Detecta inicios de sesión sospechosos a través de RDP o de la red e identifica el punto de inicio del ataque | Se registran cientos de intentos de inicio de sesión en servidores con alto tráfico        |
| **4625** (Failed Logon)     | Detecta ataques de fuerza bruta, password spraying                                                           | Los logs contienen muchas reservas que podrían llevar a interpretar erróneamente el evento |

> Un servidor de Windows puede generar decenas de eventos de login por minuto y cada uno de estos eventos puede componerse por muchos campos diferentes. Para leer acerca de más campos y tipos de inicio de sesión revisar [aquí](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia)

---
*Pregunta 1: Which IP performed a brute force of the THM-PC?* \
**Respuesta: `10.10.56.448`**

*Pregunta 2: Which user has been breached as a result of the attack?* \
**Respuesta: Administrador**

*Pregunta 3: What was the Logon ID of the malicious RDP login?* \
**Respuesta: `0x183C36D`**

> **NOTA** -> Filtrar por el Event ID `4624` y buscar la sección *New Logon*

### Security Log: User Management

| Event ID           | Descripción                                                  | Ataque malicioso                                                                                    |
| ------------------ | ------------------------------------------------------------ | --------------------------------------------------------------------------------------------------- |
| 4720 / 4722 / 4738 | Se ha creado/habilitado/cambiado una cuenta de usuario       | Los atacantes pueden crear una cuenta backdoor o habilitar una cuenta antigua para evitar detección |
| 4725/4726          | Una cuenta de usuario se ha inhabilitado/borrado             | Los actores maliciosos han deshabilitado cuentas SOC privilegiadas para retrasar sus acciones       |
| 4732/4733          | Un suario ha sido agregado/removido de un grupo de seguridad | Los atacantes usualmente agregan sus cuentas backdoor a grupos privilegiados como "Administrators"  |

> [!NOTE]
> Todos los eventos de administración de usuarios tienen una estructura similar y pueden ser divididos en tres partes:
> - Quién hizo la acción (Sujeto)
> - Quién fue el objetivo (Objeto)
> - En dónde fueron realizados los cambios exactamente (Detalles)

### Sysmon: Process monitoring


## Lecciones aprendidas