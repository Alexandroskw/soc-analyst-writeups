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
| Clark-Wilson  | Apunta también a alcanzar la **integridad**                            | - **Constrained Data Item (CDI)**: tipo de datos cuya integridad se quiere preservar<br>- **Unconstrained Data Item (UDI)**: Se refiere a los datos más allá de **CDI**<br>- **Transformation Procedures (TPs)**: Son operaciones programadas. Deben mantener la integridad de los **CDI**<br>- **Integrity Verification Procedures (IVPs)**: Revisan y se aseguran de la viabilidad de los **CDI** |

***
*Pregunta 1: Click on "View Site" and answer the four questions. What is the flag that you obtained at the end?* \
**Respuesta: THM{SECURITY_MODELS}**
### Task 5 — Defence-in-Depth

> Se refiere a crear un sistema de seguridad de múltiples niveles, por lo que también es llamado **Multi-Level Security** (Seguridad Multinivel)

***
*Pregunta 1: Make sure you have read the above.* \
**No se necesita respuesta**

### Task 6 — ISO/IEC 19249

| Principio *arquitectónico* | Descripción                                                                                                                                                                                      |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Domain separation          | Cada conjunto de componentes relacionados se agrupa en una sola entidad. Cada entidad tiene su propio dominio y se le puede asignar un conjunto común de atributos de seguridad                  |
| Layering                   | Un sistema estructurado en varios niveles abstractos o capas se vuelve posible imponer políticas de seguridad a diferentes niveles.                                                              |
| Encapsulation              | En la programación orientada a objetos (POO) se esconden las implementaciones de bajo nivel y se previene la manipulación directa de un objeto proveyendo métodos específicos para ese propósito |
| Redundancy                 | Se asegura de la disponibilidad e integridad                                                                                                                                                     |
| Virtualization             | El concepto es compartir un conjunto de hardware entre varios sistemas operativos. Provee capacidades de entornos de aislamiento que mejoran los límites de seguridad                            |

> [!TIP]
> **Respecto a la encapsulación** \
> De forma similar a la POO, en un sistema de gran tamaño se puede usar (o diseñar) una API (Application Programming Interface) adecuada para que la app utilizaría para acceder a la base de datos

| Principios de *diseño*                     | Descripción                                                                                                                                                                               |
| ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Least privilege                            | Establece que se debe de otorgar la menor cantidad de permisos posibles para que alguien pueda realizar su tarea y nada mas                                                               |
| Attack Surface Minimisation                | Cada sistema tiene vulnerabilidades que un atacante puede utilizar para comprometer el sistema                                                                                            |
| Centralized Parameter Validation           | Las entradas inválidas pueden se utilizadas para explotar vulnerabilidades en un sistema. La validación de parámetros es un paso necesario para asegurar el estado correcto de un sistema |
| Centralized General Security Services      | Se puede apuntar a centralizar todos los servicios de seguridad                                                                                                                           |
| Preparing for error and exception handling | Cada que se construye un sistema se debe tomar en cuenta que se producen y se seguirán produciendo errores y excepciones                                                                  |
***
*Pregunta 1: Which principle are you applying when you turn off an insecure server that is not critical to the business?* \
**Respuesta: 2**

> Se deben de desconectar servicios que no se utilizan para minimizar la superficie de ataque

*Pregunta 2: Your company hired a new sales representative. Which principle are they applying when they tell you to give them access only to the company products and prices?* \
**Respuesta: 1**

> Solo debe acceder a lo que le compete y nada más

*Pregunta 3: While reading the code of an ATM, you noticed a huge chunk of code to handle unexpected situations such as network disconnection and power failure. Which principle are they applying?* \
**Respuesta: 5**

> **Siempre** habrán nuevos errores y excepciones

### Task 7 — Zero Trust vs. Trust but Verify

| Principio        | Descripción                                                                                                                                                             |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Trust but verify | Siempre se debe de verificar incluso cuando se confíe en la entidad y su comportamiento. Verificar usualmente requiere configurar mecanismos de registro adecuados      |
| Zero Trust       | Trata a la confianza como una amenaza y en consecuencia se enfoca en las amenazas internas. Trata a la confianza como un adversario hasta que se demuestre lo contrario |

> La microsegmentación es una implementación de Zero Trust. Se refiera al diseño donde un segmento de red puede ser tan pequeño como un simple cliente. La comunicación entre segmentos requiere autenticación, verificación de las listas de control de acceso, entre otros.

***
**No se requiere respuesta**

### Task 8 — Threat vs. Risk

| Término                        | ¿Qué hace?                                                                                                                            |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------- |
| Vulnerability (Vulnerabilidad) | Significa susceptible a un ataque o daño. (En seguridad de la información, una vulnerabilidad es una debilidad)                       |
| Threat (Amenaza)               | Es un peligro potencial asociado con la debilidad o vulnerabilidad                                                                    |
| Risk (Riesgo)                  | Se refiere a la probabilidad de que un actor malicioso aproveche una vulnerabilidad y al impacto consecuente que tendrá en la empresa |
***
**No se requiere respuesta**
# Lecciones aprendidas
- La Triada CIA es la base de la seguridad y ciberseguridad en general
- Es importante recordar el acrónimo de la triada CIA: Confidenciality, Integrity y Availability
- Existe un opuesto a la triada CIA llamada triada DAD
- También es importante recordar qué significa DAD: Disclosure, Alteration y Destruction/denial
- Hay modelos enfocados en triada CIA, se recomienda investigar más en profundidad
- Con base en los modelos, se puede determinar qué objetivos en el contexto de la seguridad se quieren alcanzar
- El estándar **ISO/IEC 19249** sirve para catalogar los principios de seguridad relacionados a tecnologías de la información
- El principio Zero Trust es bastante agresivo ya que considera a la confianza interna una amenaza potencial hasta que se demuestre lo contrario
- El principio Trust but verify es más permisible pero se debe de automatizar debido a que se vuelve virtualmente imposible verificar a una sola entidad