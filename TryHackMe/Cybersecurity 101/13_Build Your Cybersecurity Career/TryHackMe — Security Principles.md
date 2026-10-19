# TryHackMe — Security Principles
**Dificultad** -> easy | **Date** -> 19-oct-2026 | **Type** -> Free \
**Sala** -> [Security Principles](https://www.tryhackme.com/room/securityprinciples)

# Introducción
Sala enfocada en los principios básicos de la seguridad como la triada CIA y su importancia en el mundo de la ciberseguridad

# Solución
### Task 2 — CIA
Antes de hacer un juicio de valor acerca de la seguridad de un sistema, se debe pensar en la triada de la seguridad

| Elemento                               | Descripción                                                                                          |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| **Confidentiality** (Confidencialidad) | Se asegura que solo las personas autorizadas tengan acceso a los datos                               |
| **Integrity** (Integridad)             | Se asegura que los datos no puedan ser alterados, se debe de detectar cualquier alteración si ocurre |
| **Availability** (Disponibilidad)      | Se asegura que el sistema o servicio siempre esté disponible cuando sea necesario                    |

> [!IMPORTANT]
> **Más allá de la triada CIA** \
> Hay dos elementos que son igual de importantes de tener en cuenta
> - **Authenticity** (autenticidad): Se refiere a que no es fraudulento ni falsificado. Se asegura que los documentos, archivos o datos sean de la fuente solicitada
> - **Nonrepudiation** (no repudiación): Significa negarse a reconocer la validación de algo. Se asegura que la fuente original no pueda negar que de ahí vienen los datos, documentos o archivos.

> [!TIP]
> **Parkerian Hexad** \
> Fue propuesto por Donn B. Parker en 1998 agregando otros 3 elementos a la triada CIA original, teniendo un total de 6 elementos
> - Confidentiality
> - Integrity
> - Availability
> - Utility
> - Authenticity
> - Possession

> **Authenticity** ya fue vista más arriba en esta misma tarea

| Elemento                  | Descripción                                                                             |
| :-----------------------: | --------------------------------------------------------------------------------------- |
| **Utility** (utilidad)    | Se enfoca en el uso de la informació                                                    |
| **Possession** (posesión) | Se enfoca en la protección de la información de acceso, copia o control no autorizados  |

***
*Pregunta 1: Click on "View Site" and answer the five questions. What is the flag that you obtained at the end?* \
**Respuesta: THM{CIA_TRIAD}**

> **NOTA**: Las preguntas cambian, no siempre son las mismas ni en el mismo orden

### Task 3 — DAD

> [!CAUTION]
> La seguridad de un sistema puede ser vulnerada de varias maneras; mediante la divulgación de datos confidenciales, alteración o destrucción de los datos


| Elemento                                          | Descripción                                                                                                     |
| ------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Disclosure** (Divulgación)                      | Lo opuesto a la confidencialidad. La divulgación de los datos confidenciales es un ataque a la confidencialidad |
| **Alteration** (Alteration)                       | Lo opuesto a la integridad                                                                                      |
| **Destruction/Denial** (Destrucción o Denegación) | Lo opuesto a la disponibilidad                                                                                  |

> [!IMPORTANT]
> Lo opuesto a la triada **CIA** es la triada **DAD**: **Disclosure**, **Alteration** y **Destruction**

***
*Pregunta 1: The attacker managed to gain access to customer records and dumped them online. What is this attack?* \
**Respuesta: Disclosure**

> **PALABRAS CLAVE** -> *dumped them online*

*Pregunta 2: A group of attackers were able to locate both the main and the backup power supply systems and switch them off. As a result, the whole network was shut down. What is this attack?* \
**Respuesta: Destruction/Denial**

### Task 4 — Fundamental Concepts of security models

> [!TIP]
> Existen al menos 8 modelos de seguridad pero en esta sala solo se tocan 3 de ellos y se enfocan en la triada CIA
> - Modelo Bell-LaPadula
> - Modelo Biba
> - Modelo Clark-Wilson

| Modelo        | Descripción                                                            | Reglas                                                                                                                                                                                                                                                                                                                                                                                              |
| ------------- | ---------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Bell-LaPadula | Apunta a alcanzar u obtener la **confidencialidad** siguiendo 3 reglas | - **Simple Security Property**: Establece que un sujeto en un nivel de seguridad inferior puede leer uno de nivel superior<br>- **Star Security Property**: Un sujeto de alto nivel de seguridad no puede escribir en un objeto de bajo nivel de seguridad<br>- **Discretionary-Security Property**: Utiliza una matriz de acceso para permitir operaciones de lectura y escritura                  |
| Biba          | Apunta a alcanzar la **integridad** siguiendo 2 reglas                 | - **Simple Integrity Property**: Un sujeto de alto nivel de integridad no puede accesar a un objeto de bajo nivel de integridad<br>- **Star Integrity Property**: Un objeto de bajo nivel de integridad no puede escribir a un objeto de alto nivel de integridad                                                                                                                                   |
| Clark-Wilson  | Apunta también a alcanzar la integridad                                | - **Constrained Data Item (CDI)**: tipo de datos cuya integridad se quiere preservar<br>- **Unconstrained Data Item (UDI)**: Se refiere a los datos más allá de **CDI**<br>- **Transformation Procedures (TPs)**: Son operaciones programadas. Deben mantener la integridad de los **CDI**<br>- **Integrity Verification Procedures (IVPs)**: Revisan y se aseguran de la viabilidad de los **CDI** |

***
*Pregunta 1: Click on "View Site" and answer the four questions. What is the flag that you obtained at the end?* \
**Respuesta: THM{SECURITY_MODELS}**
### Task 5 — Defence-in-Depth

> Se refiere a crear un sistema de seguridad de múltiples niveles, por lo que también es llamado **Multi-Level Security** (Seguridad Multinivel)

***
*Pregunta 1: Make sure you have read the above.* \
**No se necesita respuesta**

### Task 6 — ISO/IEC 19249

# Lecciones aprendidas