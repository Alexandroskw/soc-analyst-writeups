# TryHackMe — Hydra
**Dificultad** -> easy | **Date** -> 14-oct-2026 | **Type** -> Free \
**Sala** -> [Hydra](https://www.tryhackme.com/room/hydra)

# Introducción
Sala enfocada en el conocimiento y uso de Hydra, una herramienta ofensiva diseñada para "hackear" rápidamente contraseñas de inicio de sesión.

# Solución
### Task 1 — Hydra introduction

> [!NOTE]
> **Hydra** corre una lista y realiza fuerza bruta a algunos servicios de autenticación. Tiene la capacidad de realizar fuerza bruta a una gran cantidad de protocolos como: HTTP/HTTPS, Firebird, IMAP, MySQL, RDP, FTP, SNMP, SSH, entre otros

### Task 2 — Using Hydra

> Conectarse mediante OpenVPN a través de una máquina Kali.

Con base en qué protocolo se quiere hacer fuerza bruta, el comando cambia. Si se quiere hacer un ataque de fuerza bruta a FTP, el comando sería

> `hydra -l user -P passlist.txt ftp://<IP_MACHINE>`

`user` es el nombre de usuario al que queremos hacer fuerza bruta y `passlist.txt` es la lista de contraseñas con las que queremos realizarla.

| Bandera | ¿Qué hace?                                        |
| ------- | ------------------------------------------------- |
| `-l`    | especifica el nombre de usuario para el protocolo |
| `-P`    | Indica la lista de contraseñas                    |
| `-t`    | indica el número de hilos para usar               |

> Hydra también puede ejecutar fuerza bruta a formularios en páginas web. Se debe conocer qué método está haciendo la solicitud (`GET` o `POST`)

Un ejemplo de un ataque con Hydra a una página web

```bash
hydra -l <username> -P <wordlist> <IP_MACHINE> http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

| bandera               | ¿Qué hace?                                                                                         |
| --------------------- | -------------------------------------------------------------------------------------------------- |
| `-l`                  | Nombre de usuario del sitio web                                                                    |
| `-P`                  | Lista de contraseñas                                                                               |
| `http-post-form`      | Formulario tipo `POST`                                                                             |
| `<paht>`              | URL para inicio de sesión (`login.php`)                                                            |
| `<login_credentials>` | Nombre de usuario y contraseña usadas para el inicio de sesión (`username=^USER^&password=^PASS^`) |
| `<invalid_response>`  | Parte de la respuesta cuando el inicio de sesión falla                                             |
| `-V`                  | Modo verboso para cada intento                                                                     |
***
*Pregunta 1: Use Hydra to brute-force molly's web password. What is the value of flag 1?* \
**Respuesta: THM{2673a7dd116de68e85c48ec0b1f2612e}**

> **PRECAUCIÓN**: el archivo `rockyou.txt` viene comprimido como `rockyou.txt.gz`. Para poder utilizarlo es necesario descomprimir con `gzip -d rockyou.txt.gz`

> **NOTA**: En `<login_credentials>` utilizar `/login:username` ya que se conoce el nombre de usuario

*Pregunta 2: Use Hydra to brute-force molly's SSH password. What is the value of flag 2?* \
**Respuesta: THM{c8eeb0468febbadea859baeb33b2541b}**

```shell
# Obtener la clave ssh de Molly
hydra -l molly -P /usr/share/wordlists/rockyou.txt <IP_MACHINE> -t 4 ssh
```
# Lecciones aprendidas
- Hydra es una herramienta diseñada para romper las contraseñas por fuerza bruta
- Tiene la capacidad de romper varios protocolos
- Para evitar que se obtenga la contraseña con Hydra, es buena práctica no utilizar contraseñas [comunes](https://www.passwordmanager.com/most-common-passwords-latest-statistics/)
- Es buena práctica quitar el acceso por contraseña en SSH