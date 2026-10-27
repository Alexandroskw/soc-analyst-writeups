# TryHackMe — Introduction to EDR
**Dificultad** -> easy | **Date** -> 26-oct-2026 | **Type** -> Free \
**Sala** -> [Introduction to EDR](https://www.tryhackme.com/room/introductiontoedrs)

## Introducción
Sala de introducción a los fundamentos de un EDR, como trabaja y sus características principales.
## Solución

> [!NOTE]
> Un Endpoint Detection and Response (**EDR**) es una solución diseñada para monitorear, detectar y responder a amenazas avanzadas para proteger los endpoints de una organización.

No importa dónde esté el endpoint, el EDR siempre se asegurará de monitorearlo esté fuera del perímetro de la organización o no y las amenazas serán detectadas.

| Característica | Descripción                                                                                                                                                                                 |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Visibilidad    | Se puede ver el árbol completo de un proceso con una línea de tiempo detallada de la secuencia de acciones.                                                                                 |
| Detección      | Identifica cualquier desviación de un comportamiento esperado y la señala inmediatamente                                                                                                    |
| Respuesta      | Dentro de la consola del EDR se pueden tomar decisiones con respecto a un endpoint en concreto, como aislarlo, meter en cuarentena algunos archivos o terminar un proceso, todo remotamente |
### ¿Cómo funciona un EDR?

| Rol         | Descripción                                                                                                                                |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| Agentes     | También conocidos como sensores, son los ojos y oídos del EDR, se instalan en los endpoints                                                |
| Consola EDR | Todos los datos recolectados de los agentes es correlacionado y analizado a través de una lógica compleja y algoritmos de machine learning |

> [!TIP]
> Un EDR no está solo en un ecosistema, trabaja en conjunto con otras soluciones de seguridad y todo se centraliza en una solución SIEM que se vuelve el punto central de investigación para un analista.

### Capacidades de detección y respuesta

| Detección                          | Descripción                                                                                                                                           |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| De comportamiento                  | Observa el comportamiento completo de un archivo en lugar de solo comparar la firma con amenazas conocidas                                            |
| de anomalías                       | Con el tiempo un EDR conoce el comportamiento de un endpoint y cualquier actividad fuera de ese comportamiento es marcada                             |
| de Indicadores de compromiso (IOC) | La mayoría de los ataques tienen indicadores publicados en los canales de inteligencia sobre amenazas                                                 |
| Mapeo de MITRE ATT&CK              | Además de marcarse como maliciosa o sospechosa, la actividad también se asocia a la táctica y técnica de MITRE en la que se encuentra dicha actividad |
| Algoritmos de machine learning     | Los EDR modernos tienen modelos de machine learning entrenados con una gran base de tatos para comportamientos normales y maliciosos                  |

| Respuesta               | Descripción                                                                                                                                                   |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Aislamiento del host    | Durante una actividad maliciosa se puede aislar el endpoint de la red a través del EDR.                                                                       |
| Terminación de procesos | En algunos casos es mejor terminar el proceso de la actividad maliciosa que aislar el host completo                                                           |
| Cuarentena              | Se asegura que el archivo sea movido a una locación aislada donde no pueda ser ejecutado para posteriormente decidir si se restaura o elimina definitivamente |
| Acceso remoto           | Usualmente se ocupa cuando las soluciones dentro del ERD no son suficientes para una actividad específica                                                     |
| Colección de artefactos | Un analista puede extraer artefactos importantes de los endpoints sin la necesidad de acceder físicamente al dispositivo                                      |
## Lecciones aprendidas
- El EDR es más poderoso que un antivirus tradicional
- Si bien los EDR y antivirus en esencia son para el mismo propósito, (defender la integridad del endpoint), el EDR es muy superior en todos sentidos
- Un EDR puede diagnosticar el comportamiento de un endpoint en tiempo real
- Un EDR captura la telemetría de un endpoint
- Los EDR tienen herramientas internas que sirven para la detención de amenazas
- Se puede conectar a un endpoint a través de la consola del EDR y posteriormente descargar un archivo para su análisis o su puesta en cuarentena
- Los EDR no operan solos, se juntan con otro tipo de soluciones para tener un ecosistema completo de seguridad que termina en la centralización en la solución SIEM
- Los EDR modernos tienen algoritmos de machine learning para evitar que las amenazas puedan evadirlos