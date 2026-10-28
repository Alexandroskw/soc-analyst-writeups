# TryHackMe — Cyber Kill Chain
**Dificultad** -> easy | **Date** -> 28-oct-2026 | **Type** -> Free \
**Sala** -> [Cyber Kill Chain](https://tryhackme.com/room/cyberkillchainzmt)

## Introducción
Sala enfocada en el framework Cyber kill chain, especializado en la identificación y prevención de intrusiones de red.

## Solución
### Reconnaissance (Reconocimiento)
Los adversarios obtienen información acerca de su víctima para planificar sus siguientes pasos. (Los datos pueden incluir detalles de la infraestructura, datos de los empleados, procesos del negocio y tecnologías expuestas), usualmente este proceso es pasivo e indetectable.

> [!TIP]
> A través de **OSINT** (Open-Source Intelligence) los adversarios pueden obtener información acerca de su objetivo a través de su información pública

| Tipo                 | ¿Qué es?                                                                                                                                                 |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
|Reconocimiento Pasivo | No tiene contacto directo con el objetivo. Puede incluir búsquedas mediante WHOIS, por redes sociales o análisis de datos sobre violaciones de seguridad |
|Reconocimiento Activo | Tiene contacto directo con el objetivo con actividades como ingeniería social, escaneo de puertos, banner grabbing o cualquier otro servicio disponible  |

Algunas herramientas para hacer un reconocimiento
- [theHarvester](https://github.com/laramies/theHarvester): Es capaz de obtener nombres, subdominios, IPs y URLs, además de obtener los emails de múltiples fuentes públicas
- [Hunter](https://hunter.io/): Busca emails permitiendo obtener los datos de contacto asociados al dominio
- [OSINT Framework](https://osintframework.com/): Es una "caja de herramientas". Provee múltiples herramientas basadas en categorías

### Weaponization (Militarización)

> Los atacantes usualmente usan herramientas automatizadas para generar malware o entran a la DarkWeb para comprar el malware.

> [!WARNING]
> Actores más sofisticados o grupos **APT** (Advanced Persistent Threat) crean su malware a medida para que cada muestra sea única y evadir la detección del objetivo

En esta fase, el atacante puede seguir ciertas tácticas:
- Crear un documento de Microsoft Office infectado con una **macro** o un script de **VBA** (Visual Basic Applications)
- Crear una payload o un gusano sofisticado dentro de una USB para distribuirla al público
- Crear una infraestructura de *Command & Control* (**C2**) para ejecutar comandos en la máquina víctima o entregar más payloads
- Crear una backdoor en la víctima que le puede dar acceso y evadir los mecanismos de seguridad
- Adoptar plantillas de phishing o aplicaciones de tipo [OAuth](https://www.microsoft.com/es-mx/security/business/security-101/what-is-oauth) que parezcan legítimas

### Delivery (Entrega)
Es donde el atacante decide el método para distribuir o transmitir el malware o payload dentro del objetivo, hay varias opciones
1. **Phishing**: Después del reconocimiento, el actor malicioso puede crear un email malicioso que apunte a una persona en específico (**spear phishing**) o a múltiples personas, el email puede tener un link malicioso o un archivo adjunto que puede provocar una violación de seguridad
2. **USB Drop attack**: Un atacante puede dejar abandonado un USB en una cafetería, estacionamientos o en donde sea. También puede imprimir el logo en la USB y entregarla por correo a la empresa haciéndose pasar por un cliente que les envía un USB como regalo
3. **Watering hole attack**: Apuntan a un grupo específico de personas comprometiendo el sitio web que utilizan frecuentemente. Los redirige a una sitio malicioso diseñado por el atacante. Las víctimas pueden descargar malware sin intenciones de hacerlo o aplicaciones maliciosas en sus equipos

## Lecciones aprendidas
