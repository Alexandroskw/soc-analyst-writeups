# TryHackMe — Unified Kill Chain 
**Dificultad** -> easy | **Date** -> 30-oct-2026 | **Type** -> Free \
**Sala** -> [Unidied Kill Chain](https://www.tryhackme.com/room/unifiedkillchain)

## Introducción
Sala enfocada a la compresión del framework Unified Kill Chain, sus fases, en qué se enfoca y qué riesgos puede mitigar

## Solución

> [!NOTE]
> La "Cadena de ataque" (**Kill Chain**) tiene su origen en el ámbito militar. Describe las fases de un ataque. En el ámbito de la ciberseguridad, se refiera a la metodología o ruta que un atacante sigue para aproximarse a un objetivo

### What is "Threat modelling" (Modelado de amenazas)
Son una serie de pasos utilizados para mejora la seguridad de un sistema
- Identificar qué sistemas y aplicaciones necesitan ser asegurados y qué función tienen en el entorno donde están
- Evaluar qué vulnerabilidades y debilidades de estos sistemas pueden tener y cómo pueden ser potencialmente explotados
- Crear un plan de acción para asegurar los sistemas y aplicaciones vulnerables
- Implementar políticas de prevención para prevenir que vuelvan a ocurrir estas vulnerabilidades 

> El **Modelado de Amenazas** (**Threat Modelling**) es un paso importante ya que ayuda a reducir el riesgo dentro de un sistema o aplicación ya que crea una visión general de los activos de TI y ayuda a resolver las vulnerabilidades creando procedimientos para su resolución.

> [!TIP]
> El framework ayuda identificar posibles superficies de ataque y la forma en la que los sistemas pueden ser vulnerados. Algunos frameworks de modelado de amenazas son: **STRIDE**, **DREAD** y **CVSS**

***

La **UKC** (Unified Kill Chain) establece que hay al menos **18 fases** para describir un ataque

| Beneficios del framework UKC                                           | Otros frameworks                                                                   |
| ---------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Moderno, fue introducido en 2017 y actualizado en 2022                 | Algunos frameworks son antiguos. El entorno de la ciberseguridad era diferente     |
| Es extremadamente detallada                                            | Solo tienen algunas fases útiles                                                   |
| Cubre desde el ataque hasta la motivación de un atacante               | Solo cubren una cierta cantidad de fases                                           |
| Remarca escenarios de ataques más realistas, algunas etapas se repiten | No cuentan con que el atacante irá y vendrá en diferentes etapas durante el ataque |

***

#### Reconnaissance (Reconocimiento) ([MITRE Tactic TA0043](https://attack.mitre.org/tactics/TA0043/))
Son las técnicas empleadas por un adversario para la obtención de información relacionada al objetivo. (La información obtenida en esta fase será utilizada en fases posteriores) 

Información que se puede obtener en esta fase
- Descubrimiento de los servicios y sistemas que están en ejecución en el objetivo
- Encontrar una lista de contactos o empleados que pueden ser suplantados o utilizados para un ataque de phishing o ingeniería social
- Búsqueda de credenciales que pueden ser utilizadas en etapas posteriores
- Comprensión de la topología de red y otros sistemas de red para poder pivotar

#### Weaponization (Militarización) ([MITRE Tactic TA0001](https://attack.mitre.org/tactics/TA0001/))
El adversario está preparando el ataque y las herramientas que utilizará para dicho ataque

#### Ingeniería Social (Social Engineering) (MITRE Tactic TA0001)
El adversario utiliza técnicas para manipular a un empleado para realizar acciones que le ayuden (al atacante) a realizar el ataque:
+ Lograr que un empleado abra un archivo malicioso dentro de un email de phishing
+ Suplantar el sitio web original para que el usuario introduzca sus credenciales
+ Llamando o visitando el sitio objetivo y suplantar a un usuario o poder obtener acceso a áreas que antes no tenía acceso el atacante

#### Exploitation (Explotación) ([MITRE Tactic TA0002](https://attack.mitre.org/tactics/TA0002/))
El adversario saca ventaja de las debilidades o vulnerabilidades de un sistema
- Cargar y ejecutar un reverse shell en una aplicación web
- Interferir con un script automatizado para ejecutar código
- Abusar de una vulnerabilidad en una aplicación para ejecutar código en el sistema donde está corriendo

#### Persistence (Persistencia) ([MITRE Tactic TA0003](https://attack.mitre.org/tactics/TA0003/))
El adversario utiliza técnicas para mantener acceso al sistema al que tuvo en un inicio
- Creación de un servicio en el objetivo que le permite volver a acceder
- Agregar a la víctima a un servidor C2 para poder ejecutar comandos remotamente en cualquier momento
- Crear otro tipo de backdoors se ejecuten cada que ciertas acciones ocurran dentro del sistema

#### Defence evasion (Evasión de defensas) ([MITRE Tactic TA0005](https://attack.mitre.org/tactics/TA0005/))

> Es una de las fases más importantes en el UKC. Esta etapa es bastante útil cuando se analiza un ataque ya que ayuda a los equipos de defensa a mejorar (los equipos de defensa).

Esta fase ayuda a comprender las técnicas que el adversario utilizó para evadir las medidas defensivas como: firewalls, antivirus en la máquina del laboratorio e IDSs.

#### Command & Control (C2) ([MITRE Tactic TA0011](https://attack.mitre.org/tactics/TA0011/))
Es el resultado de los esfuerzos que hizo el adversario en la fase de "Weaponization" para establecer comunicación entre él (el adversario) y la víctima. Gracias al C2, el adversario puede: ejecutar comandos, robar datos o credenciales (otro tipo de información) y pivotar a otros sistemas a través de la víctima controlada.

#### Pivoting (Pivotar) ([MITRE Tactic TA0008](https://attack.mitre.org/tactics/TA0008/))
Es la técnica que un adversario usa para alcanzar otros sistemas en la red, que de otra forma no serían posibles al no estar conectados a Internet.

> Hay sistemas que no son alcanzables tan directamente pero contienen información útil y/o seguridad débil.


## Lecciones aprendidas
