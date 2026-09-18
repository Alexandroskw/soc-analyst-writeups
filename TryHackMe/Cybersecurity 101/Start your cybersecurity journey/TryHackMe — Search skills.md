# TryHackMe — Search skills
**Dificultad** -> easy | **Date** -> 31-ago-26 | **Type** -> Free \
**Sala** -> [Search Skills](https://tryhackme.com/room/searchskills)

## Introducción
Sala enfocada la demostración de sitios populares y servicios para la obtención de información priorizando varios elementos de ciberseguridad, como ofensiva o defensiva.

## Solución
### Task 2 — Shodan
Usualmente [Shodan](https://www.shodan.io) se describe como un motor de búsqueda del IoT pero va más allá. En sí, escanea la red para buscar equipo de red, sistemas industriales, cámaras de seguridad, cualquier cosa que esté conectada a internet de forma pública para ver qué está ocurriendo y donde está ocurriendo.

Durante una prueba de penetración o al evaluar las vulnerabilidades, Shodan es útil ya que puede mostrar las vulnerabilidades en una búsqueda. Soporta varios tipos de consulta como:

| Filtro     | Descripción                                            | Ejemplo                           |
| ---------- | ------------------------------------------------------ | --------------------------------- |
| `country`  | Limita la búsqueda a un país                           | `country:IE`                      |
| `port`     | Limita la búsqueda a un puerto o a un rango de puertos | `port:22`                         |
| `org`      | Filtra por nombres de organizaciones                   | `AS7224`<br>(Amazon Web Services) |
| `hostname` | Iguala un nombre de host o de dominio                  | `hostname:fakebank.thm`           |

___
*Pregunta 1: What domain is associated with the IP address `185.243.115.47`?* \
**Respuesta: `tryhackme.thm`**

### Task 3 — VirusTotal
[VirusTotal](https://www.virustotal.com) recolecta los resultados de al menos 70 motores de antivirus y escaneos de sitios web en una sola interfaz. Se puede subir un archivo, una URL, dominio o un hash de un archivo. Te dirá si tu búsqueda está marcado como maliciosa o no.

> [!WARNING]
> **Una herramienta no es infalible**
> 
> VirusTotal puede tener fallos, no es del todo infalible.

___
*Pregunta 1: How many security vendors have identified the file as dangerous?* \
**Respuesta: 52**

### Task 4 — [CVE](https://www.cve.org/) (Common Vulnerability and Exposures)
Como el nombre indica, son las vulnerabilidades y exposiciones mas comúnes. Es básicamente un diccionario de vulnerabilidades. Cuando se detecta una nueva vulnerabilidad se le asigna un identificador único:

> **CVE-AÑO-NÚMERO**

Si la CVE es de alto impacto, se le asignará un apodo como WannaCry o Stuxnet. Este tipo de CVE de alto impacto tienen un tipo de clasificación llamada **CVSS** (Common Vulnerability Scoring System) que tiene algunos factores a considerar.
Es un estándar para que cualquiera que discuta acerca de una vulnerabilidad esté hablando de la misma vulnerabilidad.

> [!NOTE]
> **Pruebas de conceptos (PoCs)**
> 
> Hay sitios como [ExploitDB](https://www.exploit-db.com/) que tienen scripts que demuestran estas vulnerabilidades.

___
*Pregunta 1: What **CVSS** (Common Vulnerability Scoring System) classification did the vulnerability get?* \
**Respuesta: 10**

### Task 5 — `man` (Manual or Technical documentation)
La documentación de las herramientas o productos es la forma más confiable de estar al día en el uso de las mismas. Mucho mejor que ver tutoriales por fuera.

Una de estas documentaciones son las páginas `man` en la terminal de Linux. El uso es simple: `man <comando>`. Se puede entender la herramienta sin la necesidad de buscar un tutorial por fuera.

![man_page](<./Images/nc_man_page.png>)
___
*Pregunta 1: What is the example command?* \
**Respuesta: `nc host.example.com 42`**

### Task 6 — GitHub
Es una buena fuente de información para mantenerse actualizado acerca de las últimas amenazas. Algunos investigadores suben scripts de Pruebas de Concepto (PoC), herramientas de explotación y algunos reportes detallados.

Se pueden buscar códigos de CVE para ver qué es lo que ha hecho la comunidad como: códigos PoC, scripts de escaneo o algún reporte detallado. Sin embargo, hay que tener cuidado en lo que se ejecuta porque un repositorio PoC puede ser malicioso en sí.
___
*Pregunta 1: What is the name of the script in the repository that will demonstrate the vulnerability?* \
**Respuesta: exploit.py**

> **TIP**: buscar en la sección _Usage_ del repositorio falso.

## Lecciones aprendidas
* Shodan es un "buscador" de vulnerabilidades ya que revisa en todo el internet buscando dicha query.
* VirusTotal es una herramienta poderosa para verificar la veracidad y fiabilidad de un archivo o sitio pero no es del todo infalible.
* Las **CVE** son tanto un estándar como un diccionario de vulnerabilidades.
* El comando `man` en Linux es tu amigo, no lo desperdicies.
* GitHub es más que una página para compartir código, también puede ser un lugar para investigar de las vulnerabilidades más actuales.

## Referencias
[[Network Security Essentials]]
