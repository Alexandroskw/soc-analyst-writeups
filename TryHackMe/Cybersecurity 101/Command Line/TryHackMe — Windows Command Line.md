# TryHackMe — Windows Command Line
**Dificultad** -> easy | **Date** -> 03-sep-26 | **Type** -> Free \
**Sala** -> [Windows command line](https://tryhackme.com/room/windowscommandline)
## Introducción
Sala enfocada en el uso básico de la línea de comandos en Windows.
___
*Pregunta 1: What is the default command line interpreter in the Windows environment?* \
**Respuesta: `cmd.exe`**

## Solución
### Task 2 — Basic System Information

```bash
# Acceso a la máquina
sudo openvpn <config_file>.ovpn

ssh user@<IP_MACHINE>
```

| Comando      | Descripción                                 | Equivalente en Linux |
| ------------ | ------------------------------------------- |:--------------------:|
| `set`        | Verifica la ruta desde la línea de comandos |        `env`         |
| `ver`        | Versión del S.O.                            |      `uname -r`      |
| `systeminfo` | Información detallada del S.O.              |    `hostnamectl`     |

> [!TIP]
> Para poder visualizar la información de forma más condensada se puede utilizar el comando `more` junto con un pipe `|`.

___
*Pregunta 1: What is the OS version of the Windows VM?* \
**Respuesta: 10.0.20348.2655**

> Utilizar el comando `ver` 

*Pregunta 2: What is the hostname of the Windows VM?* \
**Respuesta: WINSRV2022-CORE**

> Utilizar el comando `systeminfo`

### Task 3 — Network troubleshooting
La línea de comandos tiene, valga la redundancia, comandos relacionados con la red para revisar la configuración actual, comprobar las conexiones actuales y resolver problemas.

|  Comando   | Descripción                                                             |                       Ejemplo de uso                       |
| :--------: | :---------------------------------------------------------------------- | ---------------------------------------------------------- |
| `ipconfig` | Despliega información básica de la red.                                 |                      `ipconfig /all`                       |
|   `ping`   | Manda un paquete `ICMP` para comprobar la conexión con el servidor.     |                     `ping google.com`                      |
| `tracert`  | Traza una ruta a través de la red para alcanzar el objetivo             |                    `tracert google.com`                    |
| `nslookup` | Busca un host o dominio y devuelve su dirección IP                      | - `nslookup google.com`<br>- `nslookup google.com 1.1.1.1` |
| `netstat`  | Lista las conexiones de red actuales y los puertos que están escuchando |                      `netstat -a -i`                       |

___

*Pregunta 1: Which command can we use to look up the server’s physical address (MAC address)?* \
**Respuesta: `ipconfig /all`**

*Pregunta 2: What is the name of the service listening on port 135?* \
**Respuesta: `RpcSs`**

> **NOTA**: Revisar el puerto la imagen que está en la sala, no en la VM directamente.

*Pregunta 3: What is the name of the service listening on port 3389?* \
**Respuesta: TermService**

### Task 4 — File and disk management

|    Comando    | Descripción                                                                             | Equivalente en Linux |
| :-----------: | --------------------------------------------------------------------------------------- | :------------------: |
|     `cd`      | Moverse entre directorios. Sin argumento despliega la información del directorio actual |        `pwd`         |
|     `dir`     | Lista los directorios hijo dentro del directorio actual                                 |       `ls -l`        |
|    `tree`     | Representación visual de los directorios hijo                                           |        Igual         |
|    `mkdir`    | Crea un nuevo directorio                                                                |        Igual         |
|    `rmdir`    | Borra un directorio                                                                     |       `rm -rf`       |
|    `type`     | Despliega el contenido de un archivo de texto (`.txt`)                                  |        `cat`         |
|    `copy`     | Copia un archivo de una locación a otra                                                 |         `cp`         |
|    `move`     | Mueve un archivo de una locación a otra                                                 |         `mv`         |
| `del`/`erase` | Borra un archivo seleccionado                                                           |         `rm`         |

> [!TIP]
> Al utilizar el comando `cd` **SIN** argumento en Windows, el comportamiento es exactamente igual que el comando `pwd` en Linux.
> Sin embrago, en ambos sistemas funcionan igual para cambiar de directorio:
> `cd mi_directorio`

___
*Pregunta 1: What are the file’s contents in C:\Treasure\Hunt?* \
**Respuesta:THM{CLI_POWER}**

> **NOTA**: Utilizar `type` al final para desplegar la bandera

### Task 5 — Task and Process Management
Similar al **Task Manager** (Administrador de tareas), en la línea de comandos existe el comando `tasklist` que despliega todos los procesos activos. Se puede filtrar un proceso específico y ver sus tareas relacionadas.

> `tasklist /FI "imagename eq <PROCESS_NAME>.exe"`

___
*Pregunta 1: What command would you use to find the running processes related to notepad.exe?* \
**Respuesta: `tasklist /FI "imagename eq notepad.exe"`**

*Pregunta 2: What command can you use to kill the process with PID 1516?* \
**Respuesta: `taskkill /PID 1516`**

> **NOTA**: El comando `taskkill` tiene dos _k_.

### Task 6 — Conclusion
___
*Pregunta 1: The command `shutdown /s` can shut down a system. What is the command you can use to restart a system?* \
**Respuesta: `shutdown /r`**

*Pregunta 2: What command can you use to abort a scheduled system shutdown?* \
**Respuesta: `shutdown /a`**

## Lecciones aprendidas
+ La terminal de Windows es tan útil como la terminal de Linux.
+ Navegar entre directorios y el manejo de archivos en ambos sistemas es virtualmente igual.
+ Los comandos de ambos sistemas son iguales en algunos casos. Si se aprenden los de un sistema, los del otro serán más intuitivos.
+ Diagnosticar problemas en Windows no excluye la terminal.

## Referencias
[TryHackMe — Windows Fundamentals 2](<TryHackMe — Windows Fundamentals 2.md>)