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

## Lecciones aprendidas
