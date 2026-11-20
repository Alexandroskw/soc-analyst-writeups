# TryHackMe — Network Discovery Detection
**Dificultad** -> medium | **Date** -> 19-nov-2026 | **Type** -> Free \
**Sala** -> [Network Discovery Detection](https://www.tryhackme.com/room/networkdiscoverydetection)

## Introducción
Sala enfocada a la comprensión de como un atacante descubre activos en la red y como detectar dicha actividad.

## Solución
### Network Discovery
#### El atacante

> [!NOTE]
> Un adversario inicia su ataque tratando de descubrir activos en la organización que están al descubierto en el Internet, se le conoce como **Attack Surface** (_Ataque a superficie_)

En la fase de descubrimiento un atacante intenta obtener:
- Activos a los que se puede acceder
- IPs, puertos, Sistemas operativos y servicios corren en estos activos
- Versiones de los servicios en ejecución y si alguno de estos servicios tiene una vulnerabilidad para ser explotada

#### El defensor

> En ocasiones los defensores ejecutan software que realiza actividades de detección de redes

Durante la detección, los defensores quieren alcanzar los siguientes objetivos:
- Un inventario de los activos de la organización para asegurarse que están documetados
- Asegurarse que IPs, puertos o servicios innecesarios están abiertos y lo necesario está e ejecución
- Asegurarse que las vulnerabilidades están parchadas o al menos las vulnerabilidades que pueden ser explotadas lo estén

> [!IMPORTANT]
> Tanto atacantes como defensores realizan acciones de descubrimiento. El equipo SOC debe de distinguir entre el descubrimiento benigno del dañino.

Para poder distinguir entre un descubrimiento válido del no válido, los equipos SOC disponen de las siguientes técnicas:
- Incluir una lista de escáneres internos y externos permitidos asegurando que las alertas no se lancen en estas fuentes
- Integrar Inteligencia de Amenazas con la detección de casos de uso y marcar la actividad de escaneo solo de las fuentes maliciosas o sospechosas
- Se pudo haber pasado por alto alguna actividad de escaneo maliciosa, debido a ello algunos equipos utilizan la Inteligencia de Amenazas para elevar el nivel de severidad de las alertas en lugar de solo lanzarlas


## Lecciones aprendidas
