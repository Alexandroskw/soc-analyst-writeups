# TryHackMe — Pyramid of pain
**Dificultad** -> easy | **Date** -> 27-oct-2026 | **Type** -> Free \
**Sala** -> [Pyramid of pain](https://tryhackme.com/room/pyramidofpainax)

## Introducción
Sala enfocada en el framework de la "pirámide del dolor", qué es y como clasifica los indicadores con base en lo difícil que resulte a un adversario cambiarlos

## Solución

### Hash Values (Trivial)

> *El valor de un hash es un valor numérico de longitud fija que identifica de manera única los datos. Un valor hash es el resultado de un algoritmo de hash.*

##### Algoritmos más famosos
+ **MD5**: Ya no son considerados [criptográficamente seguros](https://datatracker.ietf.org/doc/html/rfc6151)
+ **SHA-1**: Fue inventado por la Agencia de Seguridad Nacional de los Estados Unidos y [depreciado por el NIST en 2011](https://csrc.nist.gov/news/2017/research-results-on-sha-1-collisions)
+ **SHA-2**: Fue diseñado por el NIST y la NSA en 2001 en reemplazo del SHA-1

> [!CAUTION]
> Un hash no se considera seguro criptográficamente si dos archivos tienen el mismo valor de hash.

> [!IMPORTANT]
> Modificar incluso un bit de un archivo cambia completamente el valor del hash.

### IP address (Easy)
Una táctica defensiva muy común es bloquear, descartar o rechazar las peticiones IP entrantes a través del firewall. No es a prueba de balas, el atacante simplemente puede cambiar su IP pública y volver a empezar el ataque.

> [!TIP]
> **Fast Flux** es una técnica DNS utilizada por botnets para ocultar phishing, entrega de malware y comunicación de malware a través de hosts comprometidos que actuan como proxies.

### Domain names (Simple)
Para un atacante modificar el nombre de dominio puede ser más doloroso porque tiene que comprar, probablemente, primero el dominio para posteriormente registrarlo y modificar el registro DNS.

> Los proveedores de DNS tienen bajos estándares y proveen APIs para hacer más fácil al atacante cambiar el dominio

> [!NOTE]
> Un acortador de URL es una herramienta que crea una URL pequeña y única que redirige a un sitio web específico que se configura en el acortador. Los atacantes suelen utilizar estos acortadores para ocultar sus dominios maliciosos.

#### Ver las conexiones a través de `any.run`
`any.run` es un sevicio de sandbox que ejecuta una muestra para revisar cualquier conexión como una solicitud HTTP o DNS o el proceso de comunicación con una IP en general.

| Pestaña       | Descripción                                                        |
| ------------- | ------------------------------------------------------------------ |
| HTTP Requests | registra las solicitudes HTTP desde que se ejecutó la muestra      |
| Connections   | muestra cualquier comunicación desde que se ejecutó la muestra     |
| DNS Requests  | muestra las solicitudes DNS hechas desde que se ejecutó la muestra |
### Host artifacts (annoying)
Son los rastros u observables que los atacantes van dejando tras un ataque, como valores en el registro, ejecución de procesos sospechosos, patrones de ataque o indicadores de compromiso (IOC), archivos dejados por aplicaciones maliciosas.

### Network artifacts (Annoying)
Pueden ser cadenas User-Agent, información C2 o patrones URI seguidos por una solicitud HTTP POST. En este nivel se puede detectar y responder a una amenaza y el atacante podría necesitar más tiempo para regresar y hacer cambios y cambiar sus tácticas o modificar las herramientas

> [!NOTE]
> Los artefactos de red pueden ser detectados por PCAPs como Wireshark o TShark, o explorar un IDS como Snort o Suricata

### Tools (Challenging)
Un atacante puede darse por vencido tratar de entrar en la red en esta etapa. Puede regresar y tratar de crear una nueva herramienta para volverlo a intentar o incluso tratar de encontrar una herramienta similar e invertir tiempo en pulir sus habilidades en ella.
 
> Los atacantes pueden crear documentos con macros maliciosas (spearphishing), una backdoor para establecer un C2 o crear un `.exe` o `.dll` personalizados, payloads o crackers de contraseñas

#### Algunos sitios útiles
- [MalwareBazaar](https://bazaar.abuse.ch/) y [Malshare](https://www.malshare.com): proveen muestras, fuentes maliciosas y resultados YARA
- [SOC Prime](https://tdm.socprime.com/login): una plataforma donde profesionales comparten sus propias reglas de detección de diferentes tipos de amenazas (incluyendo las últimas CVE)
- [Fuzzy hashing](https://ssdeep-project.github.io/ssdeep/index.html): Hace una comparación entre dos archivos con diferencias mínimas basándose en los valores del fuzzy hash

### TTPs (Though)

> **TTP** es el acrónimo para: **Tactics, Techniques and Procedures**

Incluyen a la Matriz MITRE ATT&CK, que se refiere a todos los pasos que un adversario sigue para alcanzar su objetivo que van desde los intentos de phishing, hasta la persistencia y exfiltración de datos.

> [!IMPORTANT]
> Si se responde con rapidez a un TTPs un atacante podría quedarse sin ninguna oportunidad de contraatacar

> [!NOTE]
> Actualmente existen 10 técnicas de exfiltración según la matriz [MITRE ATT&CK](https://attack.mitre.org/)

## Lecciones aprendidas

- La pirámide del dolor es una herramienta para hacerle la vida más complicada a un atacante
- Entre más alto escale la pirámide el atacante, más molesto le resultará atacarnos
- Hay puntos donde se puede detener a un atacante para que revalúe su táctica o se rinda y se vaya
- Los proveedores de DNS son (indirectamente) responsables de ataques punycode ya que no tienen buenas regulaciones para cambiar los nombres de dominio
- Hay plataformas donde otros profesionales comparten sus reglas para muchos tipos de ataques, no es explícitamente obligatorio crearlas desde cero
- La Matriz MITRE ATT&CK es una suerte de base de datos acerca de las tácticas y técnicas de los adversarios que se basa en observaciones del mundo real