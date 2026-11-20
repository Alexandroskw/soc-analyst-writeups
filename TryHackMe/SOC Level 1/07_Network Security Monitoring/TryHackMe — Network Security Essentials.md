# TryHackMe — Network Security Essentials
**Dificultad** -> easy | **Date** -> 18-nov-2026 | **Type** -> Free \
**Sala** -> [Network Security Essentials](https://tryhackme.com/room/networksecurityessentials)

## Introducción
Sala enfocada en los conceptos básicos de la seguridad de una red y cómo monitorear y protegerse contra los adversarios.

## Solución
### Task 3 — Una red
Una red de computadoras es una organización estructurada donde los activos de una red se conectan para establecer comunicación, compartir recursos y conectarse entre sí, y el mundo.

| Tipo                                         | Descripción                                                                                                                                                                                                                                                                       | Importancia                                                                                                                                                                                                                                                                                                             |
| -------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Endpoints                                    | Son el principal punto de entrada para los atacantes, usualmente a través de correos de phishing o descargas maliciosas.                                                                                                                                                          | Los Logs pueden revelar procesos maliciosos pero los logs de red pueden mostrar primero conexiones C2                                                                                                                                                                                                                   |
| Servidores de archivos y bases de datos      | Proveen acceso centralizado para compartir documentos mientras que la base de datos administra los datos estructurados como registros de clientes, información de RR.HH. o datos financieros                                                                                      | Los atacantes usualmente apuntan a estos servidores porque comprometerlos significa acceso a datos sensibles o valiosos.                                                                                                                                                                                                |
| Servidores de aplicación                     | Proveen servicios que los empleados y clientes utilizan a diario<br>- Sevidores web: almacena el sitio web de la empresa y su app web<br>- Servidores email: Administra las comunicaciones corporativas<br>- Gateways de VPN: Permite el acceso remoto seguro a recursos internos | Al estar en el "exterior", son objetivos de alto valor. Los atacantes los escanean constantemente por vulnerabilidades de software o configuraciones débiles.                                                                                                                                                           |
| Active Directory/Servidores de autenticación | Es la columna vertebral de muchas redes empresariales. Administra usuarios, grupos, computadoras y sus derechos de acceso.                                                                                                                                                        | - AD es el componente principal que controla a todas las cuentas de usuario y los sistemas en la red<br>- Los atacantes usualmente apuntan a AD por escalada de privilegios, persistencia y movimientos laterales<br>- Un simple dominio de administrador comprometido puede tirar toda la empresa                      |
| Róuters y switches                           | Conectan diferentes redes, como la LAN de la empresa con internet. Los switches conectan dispositivos en la misma red asegurando que las PCs, impresoras y servidores se comuniquen sin problemas.                                                                                | Aunque no están expuestos directamente, en muchas configuraciones empresariales, si son comprometidos pueden darle a los atacantes la habilidad de:<br>- Interceptar y manipular tráfico (Man-in-the-Middle)<br>- Crear backdoors para reenrutar el tráfico<br>- Abrir canales ocultos al Internet                      |
| Firewalls (dispositivos perimetrales)        | Es la principal puerta de seguridad que controla el tráfico entre la red interna y el Internet. Inspecciona los paquetes entrantes y salientes y decide entre si permitir o bloquearlos con base en las reglas establecidas.                                                      | - Proteger la empresa de exposición directa con el Internet<br>- Prevenir acceso no autorizado a servicios internos<br>- Registrar todos los intentos de conexión exitosos o bloqueados. Estos logs usualmente son los indicadores tempranos de ataques como escaneo de puertos, intentos de fuerza bruta o explotación |

### Task 4 — Network visibility
La visibilidad en una red es esencial ya que es la habilidad de monitorear y entender qué está pasando en la red. La visibilidad efectiva permite la detección de amenazas, investigación de incidentes y una fuerte postura de seguridad. \
La visibilidad efectiva permite:
- **Detectar anomalías**: Avistar patrones inusuales que pueden indicar un ataque
- **Investigar incidentes**: Reconstruir los eventos de un ataque para entender qué pasó
- **Búsqueda de amenazas**: Búsqueda proactiva de adversarios ocultos en la red
- **Asegurar el cumplimiento**: Conocer los requerimientos regulatorios registrando y monitoreando la actividad de la red 

#### Host-Centric Logs
Son generados individualmente por los dispositivos en la red como los servidores, workstations y laptos. Dan una base detallada de lo qué está pasando en un dispositivo específico.

#### Key Host-Centric Log Sources
- **Logs del S.O.**: Son los eventos registrados como inicios de sesión de usuarios, creación de procesos, inicios de servicios e intentos fallidos de inicio de sesión (Windows Event Logs, Linux `syslog`, macOS logs)
- **Logs de aplicación**: Logs del software corriendo en el host como servidores web (Apache o NGINX), bases de datos (PostgreSQL, MySQL), entre otros
- **Logs de herramientas de seguridad**: logs de software antivirus, agentes EDR y HIDS (Host-Based Intrusion Detection)

#### Network-Centric Logs
Son generados por los dispositivos de red que se colocan en la red y monitorean el tráfico que fluye a través de ellos. Muestran la IP de origen y destino, puertos, protocolos y la acción tomada (bloqueado o permitido).

> Pueden revelar los intentos iniciales de reconocimiento de un atacante, movimientos laterales o intentos de exfiltración de datos

#### Key Network-Centric Log Sources
- **Firewalls**: Proveen el registro de cada conexión permitida o denegada basada en reglas de seguridad predefinidas
- **IDS/IPS**: Monitorean el tráfico de red por patrones que coincidan con ataques maliciosos (firmas) o comportamiento inusual (anomalías). Son críticos para detectar ataques activos en tiempo real
- **Róuters y switches**: No son registros en el sentido tradicional, estos datos resumen las interacciones de tráfico, indicando quién se comunicó con quién, durante cuánto tiempo y cuánta información se envió.
- **Web proxies**: Son una mina de oro para las organizaciones que enrutan su propio tráfico web a través de proxies. Registran cada visita de los usuarios al sitio web, lo que permite detectar amenazas en línea, infracciones de políticas o intentos de fuga de datos.
- **VPN**: Administran el acceso remoto de los empleados. Sus logs registran quien está conectado a la red corporativa, desde donde y a qué hora, lo que es esencial para monitorear la seguridad de las conexiones remotas

### Task 5 — Network perimeter
Es la línea que separa a la red interna (segura) de la organización con la red externa (insegura), es decir, el Internet. 

> El perímetro de la red es la principal línea de defensa y la más importante.

#### El perímetro
Se define por componentes de hardware en las fronteras de la red. Algunos componentes del perímetro de la red incluyen:
- Firewalls
- Routers/gateways
- Zona desmilitarizada (DMZ)
- VPNs/Gateways de acceso remoto

> [!IMPORTANT]
> El perímetro de la red es una colección de dispositivos que actúan entre sí para proteger los activos de la organización. Estos dispositivos tienen un rol específico en la administración, filtrado y aseguramiento en el flujo de los datos entre la red interna y externa.

Si el perímetro está mal configurado o es débil, los atacantes pueden:
- Explotar los servicios expuestos para ganar acceso
- Realizar escaneos y reconocimiento para mapear la red
- Lanzar ataques de fuerza bruta contra servicios de inicio de sesión
- Usar canales de exfiltración de datos para enviar los datos robados

#### Rol de un analista de seguridad
- Revisar los logs de un firewall para bloquear o permitir conexiones
- Identificar intentos de escaneo o intentos de inicio de sesión por fuerza bruta
- Marcar tráfico saliente extraño que puede significar señales de malware o exfiltración de datos
- Entender qué debería (o que no debería) estar expuesto en el perímetro 

### Monitoreo y protección
Se utilizan firewalls, IDS/IPS y controles de acceso para examinar y limitar la exposición de sistemas sensibles y reforzar las reglas de seguridad; permitiendo a los analistas:
- Detectar ataques en fases tempranas como escaneo de puertos o ataques de fuerza bruta
- Detectar configuraciones erróneas que dejan a los sistemas sensibles expuestos
- Identificar anomalías del tráfico saliente que pueden indicar malware o exfiltración de datos

#### Puntos para tener en cuenta
- El trabajo de un analista es separar el tráfico normal de la actividad sospechosa
- Revisar patrones sospechosos
- Repetición de una sola fuente a varios destinos = **escaneo**
- Repetición de una sola fuente a un destino = **fuerza bruta**
- Tráfico en intervalos regulares perfectos = **Señales de malware**
- El contexto es importante. Una alerta del IDS te dice por qué algo se ha marcado es mas valioso que un bloqueo del firewall
- El monitoreo del perímetro de la red es la primera línea de defensa para detectar los ataques

---

*Pregunta 1: Examine the firewall logs. Which IP address is performing the port scan?* \
**Respuesta: `203.0.113.10`**

*Pregunta 2: In the WAF Logs, which single source IP is responsible for all the blocked web attacks?* \
**Respuesta: `198.51.100.12`**

> **NOTA** -> Utilizar `grep`

*Pregunta 3: In the VPN logs, how many brute-force attempts failed?* \
**Respuesta: 90**

> **TIP** -> utilizar `grep` con un pipe (`|`) con `wc`

*Pregunta 4: Which suspicious IP address was found attempting the brute-force attack against the VPN gateway?* \
**Respuesta: `45.137.22.13`**

---

### Investigando brechas

*Pregunta 1: Examine the firewall logs. What external IP performed the most reconnaissance?* \
**Respuesta: `203.0.113.45`**

> **TIP** -> utilizar el filtro `src_ip` en Splunk en la barra lateral izquierda

*Pregunta 2: In the firewall log, Which internal host was targeted by scans?* \
**Respuesta: `10.0.0.20`**

> **RECORDATORIO** -> filtrar primero por la IP de la anterior pregunta (`src_ip`) y después por la acción BLOCK (`action=BLOCK`)

```
action=BLOCK src_ip="203.0.113.45"
```

*Pregunta 3: Which username was targeted in VPN logs?* \
**Respuesta: `svc_backup`**

> **TIP** -> filtrar por el nombre de usuario en la query

*Pregunta 4: What internal IP was assigned after successful VPN login?* \
**Respuesta: `10.8.0.23`**

> **RECORDATORIO** -> filtrar por la IP que más ataques ha realizado y posteriormente por el acceso exitoso a la VPN

```
username=svc_backup result="SUCCESS" src_ip="203.0.113.45"
```

*Pregunta 5: Which port was used for lateral SMB attempts?* \
**Respuesta: 445**

> **TIP** -> filtrar primero por el tipo de alerta y posteriormente el puerto de destino

```
alert="ET EXPLOIT Possible MS-SMB Lateral Movement" dst_port=445
```

*Pregunta 6: In the IDS logs, which host beaconed to the C2?* \
**Respuesta: `10.0.0.60`**

*Pregunta 7: During the investigation, which IP was observed to be associated with C2?* \
**Respuesta: `198.51.100.77`**

*Pregunta 8: Which host showed the exfiltration attempts?* \
**Respuesta: `10.0.0.51`**

> **TIP** -> Revisar los logs del IDS y filtrar por la clasificación de la alerta

## Lecciones aprendidas
- La seguridad perimetral es la primer línea de defensa en una organización
- Si se configura mal el perímetro (un firewall, puertos abiertos, etc), es una ventana abierta a los ataques externos
- Al ser la primer línea de defensa, (la seguridad perimetral) da datos acerca de qué está ocurriendo en la red, como un escaneo de puertos o un ataque de fuerza bruta
- Los principales puntos de ataque son los endpoints, ya que son las computadoras de los usuarios/empleados
- Al investigar una brecha, se puede hacer mediante la terminal o herramientas que centralizan los logs como Splunk
- Un analista de seguridad es el explorador, avista las potenciales amenazas en el perímetro
- Si una IP se dirige a varios destinos es un sinónimo de un escaneo
- Si una IP se dirige a un solo destino es un sinónimo de un ataque de fuerza bruta
- Los logs de un firewall dan el contexto para empezar a hacer una investigación más profunda
- Si hay tráfico saliente que se comporta de forma anómala, puede significar una exfiltración de datos o un ataque de malware
- Cada archivo de log cuenta una parte de la historia, el trabajo del analista es hacer la relación y armar la historia completa
- Configurar bien el perímetro da la información necesaria para detectar cierto tipo de ataques en una fase temprana de los mismos