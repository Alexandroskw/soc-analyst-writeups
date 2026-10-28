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

## Lecciones aprendidas
