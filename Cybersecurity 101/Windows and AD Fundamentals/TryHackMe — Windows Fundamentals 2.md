**Dificultad** -> easy | **Date** -> 01-sep-26 | **Type** -> Free
**Sala** -> [Windows Fundamentals 2](https://tryhackme.com/room/windowsfundamentals2x0x)
## Introducción
Sala enfocada en los fundamentos de Windows como la configuración de UAC, monitoreo de recursos y el registro de Windows.
## Solución
### Task 2 — System configuration and Advance System Settings
#### System Configuration
La herramienta **MSConfig** es una forma avanzada de solucionar problemas, el proposito principal es detectarlos al iniciar el dispositivo.

> Se necesitan privilegios de administrador para poder utilizar **MSConfig**.

| Nombre       | Descripción                                                                                                                        |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| **General**  | Se puede seleccionar los servicios que Windows va a cargar al arrancar.                                                            |
| **Boot**     | Se pueden definir varias opciones de arranque para el S.O.                                                                         |
| **Services** | Lista todos los sevicios del sistema independientemente de su estado (ejecutando o detenido).                                      |
| **Startup**  | Se debe de utilizar el Administrador de Tareas (Task Manager) ya que el MSConfig NO es un administrador de aplicaciones de inicio. |
| **Tools**    | Da una breve descripción de cada herramienta en el sistema.                                                                        |
> [!INFO]- Con relación a Startup
> En la VM de THM, se esta utilizando Windows Server no Windows 10 u 11. En la pestaña de **Startup** no aparecerá nada relacionado al Task Manager.

#### Advance System Settings
Windows da unas configuraciones adicionales para controlar el comportamiento del desempeño y la recuperación.

> En la barra de búsqueda poner `View advance system settings`.

Windows utiliza un archivo de paginación para utilizar memoria virtual cuando la RAM física se llena.

> Se puede configurar el archivo de paginación en `Advance > Performance > Settings`

El tamaño predeterminado del archivo de paginación es de 1048 MB, en esta pestaña se puede cambiar el tamaño inicial, el tamaño máximo y en qué unidad se va a almacenar, y si el sistema lo gestionará automáticamente.

Windows puede crear un archivo de recuperación de fallos (crash dump file) siempre que encuentre un error crítico (una pantalla azul de la muerte, por ejemplo). Este archivo puede ayudar a un administrador o analista a entender qué salió mal.

> `Advance > Startup and Recovery > Settings`

El desplegable `Write debugging information` dice el tipo de archivo crash dump está configurado. Soporta varios tipos: **Automatic memory dump**, **Kernel memory dump**, **Small memory dump (256 KB)**, **Complete memory dump** y **Ninguno**.
___
*Pregunta 1: What is the name of the service that lists Systems Internals as the manufacturer?*
**Respuesta: PsShutdown**
> **NOTA**: Ordenar por `Manufacturer` y marcar la casilla `Hide all Microsoft services`.

*Pregunta 2: Whom is the Windows license registered to?*
**Respuesta: Windows User**
> Buscar en la pestaña de `Tools` la herramienta `About Windows`.

*Pregunta 3: What is the command for Windows Troubleshooting?*
**Respuesta: C:\Windows\System32\control.exe /name Microsoft.Troubleshooting**
> **NOTA**: Buscar en la pestaña `Tools`

*Pregunta 4: What command will open the Control Panel? (The answer is  the name of .exe, not the full path)*
**Respuesta: `control.exe`**
> Revisar la descripción de la herramienta en la pestaña de `Tools`.
### Task 3 — Change UAC Settings
El **U**ser **A**ccount **C**ontrol (**UAC**) puede ser desactivado por completo pero no se recomienda. Es un slider con 4 niveles de configuración; cada nivel cambia el cómo es que Windows da las alerta si una aplicación quiere hacer modificaciones a nivel de sistema.

| Caterogría                 | Descripción                                                                                                                           |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| **Always notify**          | El más alto nivel de seguridad. Alerta incluso cuando tu quieres hacer modificaciones.                                                |
| **Notify for apps**        | Solo notifica cuando las apps tratan de hacer modificaciones pero no cuando se cambian las configuraciones manualmente (Por defecto). |
| **Notify without dimming** | Exactamente a lo de la anterior, pero la pantalla no se atenúa.                                                                       |
| **Never notify**           | Notificaciones apagadas, no hay ningún tipo de alerta.                                                                                |
___
*Pregunta 1: What is the command to open User Account Control Settings? (The answer is the name of the .exe file, not the full path)*
**Respuesta: `UserAccountControlSettings.exe`**

> **NOTA**: buscar en la sección de `Tools` de **MSConfig**.

### Task 4 — Computer Management
La utilidad **Computer Management** tiene 3 secciones primarias: **System tools**, **Storage** y **Services and applications**.

| Sección               | Descripción                                                                                        | Ejemplo                                                                   |
| --------------------- | -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| System tools          | Crea y gestiona tareas comúnes que la computadora puede realizar en el momento que especifiquemos. | Programar una tarea para recopilar información del sistema a cierta hora. |
| Event Viewer          | Permite ver los eventos que han ocurrido en el dispositivo.                                        | Diagnosticar si hubo un error crítico o una ejecución no autorizada.      |
| Shared folders        | Es una lista de los folders compartidos en red.                                                    | Compartir la carpeta de costos.                                           |
| Local User and Groups | Lista los usuarios y grupos en el sistema                                                          | Ver si recientemente se ha creado un nuevo usuario.                       |
| Performance           | Es una herramienta llamada **Performance Monitor**                                                 | Diagnosticar un incremento en el uso de los recursos del sistema.         |
| Device manager        | Lista todos los dispositivos que están conectados en el sistema.                                   | Habilita o deshabilita un dispositivo que está causando conflicto.        |
> [!INFO] Eventos en Windows
> Para más detalle de los tipos de eventos que se pueden registrar, revisar [Event types](https://learn.microsoft.com/en-us/windows/win32/eventlog/event-types)
> Para los logs estándar que se pueden ver bajo el Windows Logs, revisar [Eventlog Key](https://learn.microsoft.com/en-us/windows/win32/eventlog/eventlog-key)

**Storage** tiene dos partes importantes **Windows Server Backup** y **Disk Management**.

> [!NOTE]
> Como el laboratorio es Windows Server, hay herramientas que no están disponibles en Windows 10 u 11.

El **Disk Management** puede realizar tareas avanzadas en los almacenamientos como:  Configurar nueva unidad, Extender una partición, Encoger una partición, Asignar o cambiar la letra de una unidad.

Los **servicios** son un tipo especial de aplicación que se ejecutan en segundo plano.

> [!TIP]
> Otra forma de ver los servicios es: `Win + R` y escribir `services.msc`

> Para más información de un servicio `click derecho > Properties`

El menú `Startup type` tiene 3 tipos de configuración del servicio

| Configuración | Descripción                                          |
| ------------- | ---------------------------------------------------- |
| Automático    | Inicia al encender el equipo (por defecto)           |
| Manual        | Arranca cuando otro servicio o el usuario lo ejecuta |
| Desactivado   | No se ejecutará                                      |
> [!WARNING]
> La herramienta **WMIC** ha sido sustituída en Windows 10 por PowerShell.

___
*Pregunta 1: What is the command to open Computer Management?*
**Respuesta: `compmgmt.msc`**

*Pregunta 2: When is the `npcapwatchdog` scheduled task set to run at?*
**Respuesta: At system startup**

*Pregunta 3: What is the name of the hidden folder that is shared?*
**Respuesta: sh4r3dF0Ld3r**

### Task 5 — System Information
> *Windows incluye una herramienta llamada Microsoft System Information (`Msinfo32.exe`). Esta herramienta recopila información sobre la computadora y muestra una vista completa del hardware, componentes del sistema y entorno de software, que puede ser utilizada para diagnosticar problemas informáticos*.

| Sección              | Descripción                                                                                                                                                                                                                                                            |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hardware resources   | No es para el usuario promedio. Para más información revisar el [sitio oficial](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/hardware-resources#:~:text=Hardware%20resources%20are%20the%20assignable,of%20bus%2Drelative%20memory%20addresses.). |
| Components           | Despliega información específica del hardware de la computadora.                                                                                                                                                                                                       |
| Software environment | Información acerca del software que viene con el S.O. y el que se ha instalado. Otra información útil es **Environment Variables** y **Network Connections**                                                                                                           |

Las variables de entorno almacenan infomación del entorno del sistema operativo. También almacenan datos que son usados por el S.O. y otros programas.

> La variable de entorno `WINDIR` almacena la localización del directorio de instalación de Windows.

> [!TIP] Otras maneras de encontrar las variables de entorno
> `Control Panel > System and Security > System > Advanced system settings > Environment Variables`
> 
> `Settings > System > About > system info > Advanced system settings > Environment Variables`

___
*Pregunta 1: What is the command to open System Information? (The answer is the name of the .exe file, not the full path)*
**Respuesta: `msinfo32.exe`**

*Pregunta 2: What is listed under System Name?*
**Respuesta: `THM-WINFUN2`**
> **NOTA**: revisar en `System Summary`.

*Pregunta 3: Under Environment Variables, what is the value for ComSpec?*
**Respuesta: `%SystemRoot%\system32\cmd.exe`**
### Task 6 — Resource Monitor
A diferencia de otras herramientas mencionadas anteriormente, esta está enfocada en usuarios avanzados que necesitan resolver problemas avanzados en el sistema.
La pestaña **Overview** tiene cuatro secciones: **CPU**, **Disco**, **Red** y **Memoria**.
Las pestañas consecuentes corresponden a una de estas secciones de forma más específica.

> [!INFO]
> Hay un panel en el extremo derecho de **Resource Monitor** que muestra una vista
> gráfica en tiempo real de cada una de las secciones.

___
*Pregunta 1: What is the command to open Resource Monitor? (The answer is the name of the .exe file, not the full path)*
**Respuesta: `resmon.exe`**

> **NOTA**: la misma sala da el nombre al inicio, no es necesario encender la VM.
### Task 7 — Command prompt
La línea de comandos (CMD) sigue siendo útil hoy día incluso cuando todo es a través de una interfaz gráfica (GUI).

| Comando    | Descripción                                             |
| ---------- | ------------------------------------------------------- |
| `hostname` | Despliega el nombre de la computadora                   |
| `whoami`   | Despliega el nombre del usuario actual                  |
| `ipconfig` | Despliega las configuraciones de red                    |
| `cls`      | Limpia la línea de comandos                             |
| `netstat`  | Despliega las estadísticas de TCP/IP actuales de la red |
| `net`      | Administra los recursos de la red                       |
Similar al comando `man` en Linux, los comandos en Windows también tienen un manual de ayuda que se puede desplegar con: `/?`
> `ipconfig /?`

> [!INFO]
> Habrá comandos (como `net`) que no funcionará el `/?`, en este caso, la palabra
> `help` es la que desplegará el modo de uso del comando: `net help user`

> [!TIP] ¿Qué comandos puedo ejecutar?
> Para ver todos los comandos que se pueden ejecutar en el CMD, revisar [aquí](https://ss64.com/nt/).

___
*Pregunta 1: In System Configuration, what is the full command for Internet Protocol Configuration?*
**Respuesta: `C:\Windows\System32\cmd.exe /k %windir%\system32\ipconfig.exe`**

*Pregunta 2: For the ipconfig command, how do you show detailed information?*
**Respuesta: `ipconfig /all`**
### Task 8 — Registry Editor
El Editor de Registro contiene información que constantemente se referencia durante la operación del S.O como:
+ **Perfiles por cada usuario**
+ **Aplicaciones instaladas y el tipo de documentos que generan cada una**
+ **Configuración de la hoja de propiedades para carpetas e iconos de aplicaciones**
+ **Qué hardware tiene el equipo**
+ **Puertos que estan siendo usados**

> [!CAUTION]
> El registro es exclusivamente para usuarios avanzados. El modificarlo
> puede afectar al comportamiento de todo el sistema.

___
*Pregunta 1: What is the command to open the Registry Editor? (The answer is the name of  the .exe file, not the full path)*
**Respuesta: `regedt32.exe`**

> **NOTA**: no es `regedIt32.exe`, se debe quitar la _i_.

## Lecciones aprendidas
+ `msconfig` es mas poderoso de lo que aparenta. Se debe revisar a detalle la pestaña de herramientas.
+ El **UAC** configurado por defecto es más que suficiente en la mayoría de los casos.
+ **Computer Management** es una forma de monitorear a los usuarios y qué se comparte entre ellos. También ayuda a crear tareas programables que pueden resultar repetitivas.
+ Las variables de entorno tienen la información necesaria para el comportamiento de Windows y, el hardware y software que se instala en él.
+ El monitor de recursos es un **Task Manager** con esteroides. Útil para corroborar problemas en el dispositivo.
+ `/?` o `help` son útiles cuando no se sabe la sintaxis o el funcionamiento de un comando; equivalente a `man` en Linux.
+ El editor de registro es altamente sensible. No se recomienda entrar a no ser que sea de extrema necesidad.
+ La mayoría de los comandos se pueden ejecutar con `Win + R`.