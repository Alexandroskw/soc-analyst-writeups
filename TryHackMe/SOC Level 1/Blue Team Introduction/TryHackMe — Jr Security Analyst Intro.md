# TryHackMe — Jr Security Analyst Introduction
**Dificultad** -> easy | **Date** -> 20-oct-2026 | **Type** -> Free \
**Sala** -> [Jr Security Analyst Introduction](https://tryhackme.com/room/jrsecanalystintrouxo)

# Introducción
Sala de introducción al día a día de un **Analista SOC L1** (jr security analyst)

> [!NOTE]
> Se puede tener novedades de las últimas noticias en ciberseguridad [aquí](https://thehackernews.com/)

Algunas tareas diarias de un **Analista SOC L1**
+ Detectar y prevenir el robo de datos en la laptop de un compañero
+ Analizar y detener campañas de phishing al departamento de finanzas
+ Participar en un gran incidente como un ataque de ransomware a gran escala
+ Hacer equipo con tus compañeros para construir reglas de detección y automatizaciones
+ Entender cómo es que las compañías operan desde dentro
### El equipo SOC

| Rol                            | Qué hace                                                       | Cuándo interactúo con él                          |
| ------------------------------ | -------------------------------------------------------------- | ------------------------------------------------- |
| **Analista senior**            | Ayuda a los analistas junior y atiende los casos complejos     | Cuando escalo una alerta tras el análisis inicial |
| **Ingeniero SOC**              | Mantiene las herramientas de seguridad y configura las alertas | Cuando una alerta genera ruido o necesita ajuste  |
| **Manager del SOC**            | Supervisa al equipo y reporta resultados a la dirección        | De forma general, en la gestión del equipo        |
| **Respondedor de incidentes**  | Interviene bajo demanda en incidentes graves                   | Cuando se declara un incidente importante         |
## Task 2
1. El dashboard del SIEM, muestra 2 alertas críticas. Una IP sospechosa: `221.181.185.159` ha intentado conectarse múltiples veces a través de SSH, posteriormente tuvo éxito al conectarse a través de SSH.

> [!IMPORTANT]
> Tomar en cuenta que la conexión exitosa es una alerta por si misma ya que no se conoce de donde proviene la IP sospechosa

![](<./Images/A day in the life soc jr.png>)

2. Se indaga más sobre la IP sospechosa en IP Hunter

![](<./Images/IP hunter.png>)

- La IP está asociada al menos a otros 4 ataques
- La IP proviene de China. Históricamente se han reportado múltiples ataques provenientes de este país.
3. 

| Campo        | Descripción                                                                  |
| ------------ | ---------------------------------------------------------------------------- |
| Veredicto    | La actividad es sospechosa, se justifica su contención                       |
| Escalamiento | Ticket se pasa a Will Griffin (analista senior)                              |
| Acción       | Bloquear la IP a través del firewall                                         |
| Por qué      | La IP está relacionada a otros ataques. Lo mejor es evitar nuevas conexiones |
4. Se bloquea la IP pero no se puede hacer nada con la conexión que ya ha ocurrido. Posteriormente se puede
   - Hacer una investigación acerca de qué movimientos realizó
   - Indagar si es una conexión legítima pero en un entorno no conocido
   - ¿El host debe contenerse o simplemente se debe de indagar más?
   - Documentar este incidente en un playbook
## Lecciones aprendidas
- El pipeline es: alerta -> contexto -> enriquecimiento -> decisión -> escalamiento -> acción
- Una conexión exitosa (en este caso), a través de SSH es más grave que múltiples intentos fallidos de la misma
- Si bien el país de origen no es un indicativo de actividad maliciosa, es importante conocer el historial de dicha IP
- Bloqueo != solución. El bloqueo es una contención
- Se debe de indagar qué sucedió antes del bloqueo
- Un L1 no es el SOC en sí, se debe de saber a quien escalar una alerta
## Referencias
[TryHackMe — Introduction to SIEM](<../../Cybersecurity 101/11_Security Solutions/TryHackMe — Introduction to SIEM.md>)