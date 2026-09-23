# TryHackMe — Log Operations
**Dificultad** -> easy | **Date** -> 26-ago-26 | **Type** -> Free \
**Sala** -> [Log Operations](https://tryhackme.com/room/logoperations)

# Introducción
Sala enfocada en la configuración de los logs necesarios para gestionar y analizar los logs en un entorno operativo.

# Solución
## Task 2 — Log configuration
Seleccionar la configuración adecuada es esencial para la ciberseguridad en una organización. Las configuraciones se pueden dividir en 4 propósitos comúnes: **Seguridad**, **Operacional**, **Legal** y **Depuración**.

| Propósito   | Descripción                                                                                                                                                                                                                                                                   |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Seguridad   | Enfoque principal en la detección y respuesta a anomalías y problemas de seguridad asegurando la integridad de los sistemas y la confidencialidad de los datos del usuario.                                                                                                   |
| Operacional | Enfoque principal en la detección y respuesta a errores del sistema para identificar puntos de acción para aumentar el desempeño, continuidad y fiabilidad del sistema.                                                                                                       |
| Legal       | Similar a los propósitos de seguridad. El enfoque principal es incrementar el alineamiento con las regulaciones. Las leyes y cumplimiento de estándares varía dependiendo del alcance del trabajo, el cómo se procesan los datos y el tipo de área de servicio proporcionada. |
| Depuración  | Enfoque principal en mejorar la fiabilidad del sistema descubriendo bugs y fallas potenciales. Se implementa más en entornos de prueba y desarrollo más que en entornos de producción.                                                                                        |

> [!NOTE]
> Ejemplo de una norma en el propósito legal \
> **ISO 27001**.

___
*Pregunta 1: Which of the given log purposes would be suitable to measure the cost of using a service?* \
**Respuesta: Operational**

> **PALABRAS CLAVE** -> *cost of using a service*

*Pregunta 2: Which of the given log purposes would be suitable for investigating application logs for enhancement and stability?* \
**Respuesta: Debug**

> **PALABRA CLAVE** -> *application*
___
## Task 3 — Where To Start and What To Do After Deciding the Log Purpose
Es una buena idea hacer reuniones y lluvia de ideas. Las reuiniones pueden parecer algo superfluo pero puede ser el desencadenante de una lluvia de ideas.
Hacer preguntas (correctas) es la forma más rápida de crear un plan al identificar las necesidades de cada configuración de los logs ya que cada una de ellas cumple un rol en específico.

> [!TIP]
> **Preguntas que se pueden hacer en una reunión**
> - ¿Qué se va a registrar[^1] y para qué?
> 	- ¿Se requiere esfuerzo o un compromiso adicional para cumplir el propósito?
> - ¿Cuánto se va a registrar? (detalles del alcance)
> - ¿Cuánto necesitas registrar?
> - ¿Cómo vas a almacenar los registros recopilados?
> 	- ¿Existe algún estándar, legislación o ley que se debe cumplir con base a los datos que se registran?
> - ¿Cómo vas a proteger los registros?
> - ¿Cómo vas a analizar los registros recopilados?
> - ¿Tienes los recursos necesarios y la fuerza de trabajo para hacer los registros?
> - ¿Tienes el presupuesto suficiente para planear, implemenetar y mantener los registros?

___
_You are a consultant working for a growing startup. As a consultant, you participated in a log configuration planning session. The company you work for is working to get compliant to process payment information. The given question set is being discussed._ \
*Pregunta 1: Which question's answer can be "as much as mentioned in the PCI DSS requirements."?* \
**Respuesta: How much do you need to log?**

> **PALABRAS CLAVE** -> *Payment information*

## Task 4 — Configuration dilemma

> **Cumplir con los requisitos operativos y de seguridad específicos (no negociables) mientras que también se considera la viabilidad de mejorar la capacidad mediante la implementación de datos e ideas adicionales.**

Se debe encontrar un equilibrio en las desiciones a nivel "operativo y de gestión" para lograr resultados seguros, eficientes, proactivos, resilientes y sostenibles en el ámbito de amenazas y de TI.
___
_The session continues, and your teammates need your help; they will negotiate for logging budget and operation details. As a consultant, you must remind them of a vital point._ \
*Pregunta 1: Which requirements are non-negotiable?* \
**Respuesta: Operational and security requirements**

> Los requerimientos operacionales son el día a día de la organización. \
> Los requerimientos de seguridad se encargan de cumplir regulciones estandarizadas para protegerse contra amenazas potenciales.

## Task 5 — Principles and difficulties
### Principios de registro

| Principios                | Descripción                                                                                                                                                                                                                                           |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Recolección               | - Definir el propósito de los registros.<br>- Recolectar lo que se va a necesitar y usar.<br>- No recolectar datos irrelevantes.<br>- Evitar el ruido de logs.                                                                                        |
| Formato                   | - Registrar en el nivel y detalle correctos<br>- Implementar un formato consistente<br>- Asegurar que el timestamp en los logs están sincronizados y son precisos.                                                                                    |
| Archivado y accesibilidad | - Definir las políticas de retención de logs e implementarlas.<br>- Almacenar los datos del log y asegurarse de que la parte importante está disponible para el análisis.<br>- Crear respaldos de los datos del log almacenados y los sistemas usados |
| Monitoreo y alertas       | - Crear alertas y notificaciones para casos importantes y dignos de mención<br>- Enfocarse en alertas accionables y evitar el ruido                                                                                                                   |
| Seguridad                 | - Proteger los logs implementando medidas de control.<br>- Implementar cifrado si se requiere<br>- Usar una solución de gestión de logs dedicada                                                                                                      |
| Cambio contínuo           | - Ser abierto a un cambio contínuo<br>- Entrenar al personal                                                                                                                                                                                          |

### Desafíos

| Desafíos                            | Descripción                                                                                                                                                                                                                                                                                                                                                                                                     |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Volúmen de datos y ruido            | - Tener múltiples fuentes de datos con las que lidiar.<br>- Diferencias en los volúmenes de registro creados por las aplicaciones.<br>    - Algunas aplicaciones generan una cantidad insuficiente de registros.<br>	- Los solicitantes a gran escala podrían generar volúmenes de registros masivos.<br>- Algunos registros pueden proporcionar datos no esenciales y dificultar el proceso de identificación. |
| Desempeño del sistema y recolección | - La recolección de registros puede ralentizar el rendimiento del sistema.<br>- Los sistemas no siempre son "de última generación".<br>    - Algunos sistemas "sensibles" o "antiguos" son imposibles de tocar.<br>- Desafíos de implementación y optimización.<br>    - Gestionar las actualizaciones de versiones del sistema y del agente y la sincronización en redes a gran escala es abrumador.           |
| Procesar y archivar                 | - Tener múltiples formatos de datos para manejarlo.<br>    - Analizar diferentes fuentes y formatos de datos es lento y propenso a errores.<br>- Equilibrar la retención de registros puede ser un desafío.<br>    - Especialmente cuando se trata de muchas regulaciones y estándares de cumplimiento.                                                                                                         |
| Seguridad                           | - Garantizar la seguridad de los datos es una tarea/desafío en sí mismo.                                                                                                                                                                                                                                                                                                                                        |
| Análisis                            | - Combinar, correlacionar y analizar datos de múltiples fuentes para entender el contexto de un incidente es un proceso que requiere mucho tiempo y requiere recursos informáticos y experiencia significativos.<br>    - Lograr esto en tiempo real es también otro desafío del mismo alcance.<br>	- Evitar los falsos positivos/negativos es abrumador.                                                       |
| Miscelanea                          | - Falta de planificación y hoja de ruta.<br>- Falta de recursos financieros/presupuesto.<br>- Falta de escenarios de implementación, guías y ejercicios.<br>- Falta de habilidades técnicas para implementar, mantener y analizar.<br>- Centrarse en la recopilación de registros en lugar de la fase de análisis.<br>- Ignorar los factores humanos y los posibles errores del sistema.                        |

___
_Your team is working on policies to decide which logs will be stored and which portion will be available for analysis._ \
*Pregunta 1: **Which of the given logging principles would be implemented and improved?*** \
**Respuesta: Archiving and Accessibility**

> **PALABRAS CLAVE** -> *stored* y *available for analysis*

_Your team implemented a brand new API logging product. One of the team members has been tasked with collecting the logs generated by that new product. The team member reported continuous errors when transferring the logs to the review platform._ \
*Pregunta 2: **In this case, which of the given difficulties occurs?*** \
**Respuesta: Process and archive**

> **PALABRAS CLAVE** -> *collect* y *review platform*

## Task 6 — Common Mistakes and Best Practices
Los registros son una herramienta valiosa y útil pero se debe de realizar una implementación y planeación sólidas ya que se pueden volver ineficientes haciendo que las cosas se vuelvan tediosas y difíciles de hacer. Los registros son operaciones en vivo y constante cambio, no se puede aplicar el famoso: "_Si funciona, no lo toques_".

> [!IMPORTANT]
> Acciones a aplicar para autoevaluación
> - Aprender de errores y fallas.
> - Hacer un seguimiento de la dinámica sectorial de amenazas para el sector operado y realizar pruebas regulares de alcance y resiliencia.
> - Seguir las mejores prácticas de los líderes y expertos del sector.

| Errores                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Mejores prácticas                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| - Registro de información sensible<br>- Crear registros por ti mismo.<br>- Tener registros no recopilados.<br>- Recoger todo pero no analizar.<br>- Recopilación de registros sin una planificación y configuración adecuadas.<br>- Tener sistemas que carecen de la configuración de registro planificada/requerida.<br>- Omitir la escala, las pruebas y el análisis de funcionalidad.<br>- Centrarse en los bordes y omitir los sistemas internos en el análisis.<br>- "Buscando lo que quieres encontrar" y "No investigando lo que ves".<br>- Olvidando que el proceso toma la forma de una planificación, gestión y análisis adecuados. | - Cree una configuración de registro adecuada y planifique según sus sistemas.<br>- Implementar pruebas en escala, funcionalidad y estabilidad operativa.<br>- Excluir el registro de información sensible!<br>- Protege tus registros.<br>- Crear alertas/notificaciones significativas.<br>- Céntrate en obtener información sobre resultados prácticos e impactantes.<br>- Capacita a tus analistas y mejora sus habilidades.<br>- Actualizar/mantener sus planes de operación y componentes/activos según sea necesario. |

___
_As a consultant, you are doing a comprehensive risk assessment and noticed that one of the development teams implemented a custom script to generate logs for an old system, which omits loggings at some phases._ \
*Pregunta 1: **What you would call this? (Mistake or Practice?)*** \
**Respuesta: Mistake**

# Lecciones aprendidas
* Identificar el propósito al configurar los logs (seguridad, operacional, legal o depuración).
* Hacer las preguntas correctas con base en el contexto de la organización ahorrará tiempo a la hora de configurar los logs.
* Identificar los requisitos operativos y de seguridad es imprescindible a la hora de crear las configuraciones.
* Se debe de tener en cuenta los principios de registros y los desafíos a la hora de crear una configuración de los logs (recolección, formato, proceso y archivo, etc.).
* Se debe tener buenas prácticas a la hora de configurar los logs, ya que si no se hace con precaución, los logs se pueden volver ineficientes. Siempre están cambiando.

# Referencias
[TryHackMe — Intro to logs](../Intro%20to%20Logs/TryHackMe%20—%20Intro%20to%20logs.md)

[^1]: *Registrar* = logging, *registro(s)* = log(s)
