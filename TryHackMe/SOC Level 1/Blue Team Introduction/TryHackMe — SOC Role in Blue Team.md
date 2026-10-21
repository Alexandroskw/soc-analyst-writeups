# TryHackMe — SOC Role in Blue Team
**Dificultad** -> easy | **Date** -> 20-oct-2026 | **Type** -> Free \
**Sala** -> [SOC Role in Blue Team](https://www.tryhackme.com/room/socroleinblueteam)

# Introducción
Sala introductoria al rol de un Analista SOC L1 en el Blue Team y con qué otros equipos trabaja.

> [!NOTE]
> Se recomienda revisar [TryHackMe — SOC Fundamentals](<../../Cybersecurity 101/10_Defensive Security/TryHackMe — SOC Fundamentals.md>)
# Solución

> [!TIP]
> El **CISO** (**C**hief **I**nformation **S**ecurity **O**fficer) es el encargado de crear los departamentos necesarios de acuerdo a la necesidad de la organización/empresa

## Departamentos de seguridad

| Equipo    | ¿Qué es?                                                                                              |
| --------- | ----------------------------------------------------------------------------------------------------- |
| Red Team  | Expertos en seguridad ofensiva, pentesters o hackers éticos que buscan errores de seguridad           |
| GRC Team  | Administran las políticas y se aseguran de cumplir con regulaciones                                   |
| Blue Team | Expertos en seguridad defensiva como el Analista SOC, ingenieros o personal de respuesta a incidentes |

> [!TIP]
> El **CERT** (o **CIRT**) es el encargado de manejar amenazas que el SOC no puede

| Rol SOC    | ¿Qué hace?                                        |
| ---------- | ------------------------------------------------- |
| L1         | Triaje de alertas y pasa casos complejos al L2    |
| L2         | Investigan ataques más avanzados                  |
| Ingenieros | Configuran las herramientas como el EDR o el SIEM |
| Manager    | El capitán del equipo                             |

> No todas las organizaciones pueden manejar su propio SOC, así que recurren a un proveedor de servicios de seguridad (**MSSP**). Provee a sus clientes soluciones de seguridad, comúnmente de tipo SOC

| Caso                   | SOC                                                                        | MSSP                                                                                          |
| ---------------------- | -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| Escenario ficticio     | En un SOC Interno se protegen los sistemas de la misma organización        | Se deben proteger a más de un cliente                                                         |
| Trabajo                | Los turnos usualmente son tranquilos y sin mucha presión                   | Los turnos empiezan con una lista de alertas urgentes para analizar                           |
| Herramientas           | Son unas cuantas herramientas especializadas que se necesitan conocer bien | Se deben conocer múltiples herramientas y plataformas                                         |
| Práctica de incidentes | Se aprende de unos pocos incidentes de seguridad                           | Cada semana hay que lidiar con varios ataques y brechas, se debe de aprender también de ellos |

> Para empezar, eligiría un **SOC** interno porque preferiría construir una base sólida y aprender unas cuantas herramientas a tener que aprender más de una al mismo tiempo 


# Lecciones aprendidas