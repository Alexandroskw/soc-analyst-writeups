# TryHackMe — SOC Simulator: Phishing de falso paquete de Amazon
**Dificultad** -> easy | **Date** -> 22-oct-2026 | **Type** -> Free \
**Sala** -> N/A

## Resumen
Se detectó un correo sospechoso suplantando la identidad de Amazon (impersonation) con el ID 8815 en el dashboard. Segundos después, se lanzó otra alerta, esta vez relacionada al firewall bloqueando la conexión a una URL desconocida (ID 8816). Ambas alertas están relacionada, ya que la URL bloqueada está dentro del cuerpo del email que se recibió primero.
## Alert-8815-Inbound email containing suspicious external link

| Item           | Description                                              |
| -------------- | -------------------------------------------------------- |
| Alert Summary  | Email phishing attempt; impersonation                    |
| First Handler  | Alejandro, L1 analyst                                    |
| Alert type     | New                                                      |
| Alert Name     | (8815) Inbound email containing suspicious external link |
| Timestamp      | 10/22/2026 22:59:08.946 (Dashboard timestamp)            |
| Alert Severity | Medium                                                   |
| Recipient      | `h.harris@thetrydaily.thm`                               |
| IP Address     | `10.100.198.144` (pending confirmation)                  |
| Detection time | 10/22/2026 23:00:29.594 (Splunk timestamp)               |

### Análisis

![8815\_amazon\_email\_impersonation](<./Images/8815_amazon_email_impersonation.png>)

Un correo sospechoso a nombre de Amazon con una terminación extraña: `urgents@amazon.biz`. Una suplantación de identidad (impersonation), los correos oficiales relacionados con Amazon terminan en `.com`. El correo afectado es: `h.harris[@]thetrydaily[.]thm`. El enlace sospechoso está en el cuerpo del correo electrónico y, a simple vista se ha utilizado un acortador de URL para evitar ver adonde redirige el enlace `hxxp://bit[.]ly/3sHkX3da12340`.

### Evidencia
1. **Motivo de la alerta**: La alerta se dispara cada que llega un correo con uno o más enlaces agregados y que son (potencialmente) peligrosos. El correo tiene una dirección sospechosa. ![](<./Images/8815_SIEM_alert.png>)
2. **Crea una necesidad de urgencia**: Insta al usuario a resolver de manera urgente un conflicto con respecto de un paquete.
3. **Análisis del emisor del correo**: Siguiendo las instrucciones del playbook, después de analizar los artefactos del correo, se debe de averiguar si el emisor es confiable. Debido a que es un correo electrónico, no se puede hacer un veredicto acerca de si es malicioso o no hasta hacerlo con el enlace en el correo.

### Veredicto
Se confirma como **Verdadero Positivo**. El correo es con seguridad una campaña de phishing donde suplantan la identidad de Amazon. La evidencia apunta que el usuario H. Harris es la víctima principal ya que no hay otro correo con el mismo enlace en otro endpoint.

## Alert-8816-Access to Blacklisted External URL Blocked by Firewall

| Item           | Description                                                   |
| -------------- | ------------------------------------------------------------- |
| Alert Summary  | External URL accessing blocked by firewall                    |
| First Handler  | Alejandro, L1 analyst                                         |
| Alert type     | New                                                           |
| Alert Name     | (8816) Access to blacklisted external URL blocked by firewall |
| Timestamp      | 10/22/2026 23:00:22.946 (Dashboard timestamp)                 |
| Alert Severity | High                                                          |
| Recipient      | `h.harris@thetrydaily.thm`                                    |
| IP Address     | `10.100.198.144`                                              |
| Detection time | 10/22/2026 23:01:43.594 (Splunk timestamp)                    |

### Análisis

![](<./Images/8816_dashboard.png>)

Poco después de la primera alerta (ID 8815), el firewall bloqueó una URL maliciosa. Esta regla se lanza cada que algún usuario intenta conectarse a una URL bloqueada dentro de la lista negra del firewall o si el análisis de amenazas lo determina así. La IP asociada a ese intento de acceso es `10.100.198.144`, (presumiblemente la IP asociada a la workstation de H. Harris). Así mismo, el enlace al que se intentó acceder, es el mismo enlace que se detectó en el correo poco antes de que se lanzara esta alerta. Por lo que se puede confirmar que H. Harris intentó acceder al enlace.

### Evidencia
1. **Motivo de la alerta**: La alerta se dispara cada que un usuario intenta acceder a un sitio que ha sido marcado como sospechoso dentro de las reglas del firewall o debido a que la inteligencia de amenazas lo ha marcado como una.
2. **Mismo enlace del correo anterior**: Segundos antes del disparo de esta alerta del firewall, llegó una alerta sobre un correo con un enlace (potencialmente) peligroso.
   ![](<./Images/8815_SIEM_alert.png>)
3. **Análisis del enlace del emisor del correo**: Siguiendo las instrucciones del playbook, se debe de verificar los detalles de la conexión y posteriormente la veracidad del enlace. ![8815\_link](8815_link.png) De acuerdo con *TryDetectThis*, el enlace está marcado como malicioso.
4. **¿Qué relación tiene con otra actividad?**: Antes de esta alerta, en el dashboard apareció una (alerta) relacionada a un correo con un enlace (potencialmente) dañino. Cuando se hace la correlación, el enlace al que se intentó ingresar, es el mismo que el del correo. ![](<./Images/8816_SIEM_alert.png>)

### Veredicto
Se confirma como un **Verdadero Positivo**, un intento de ingresar a un sitio marcado como malicioso. La evidencia apunta que el usuario H. Harris intentó ingresar desde su workstation a dicho enlace y el firewall bloqueó el intento de conexión.

## Línea de tiempo

| Evento                         | timestamp del dashboard           | timestamp de Splunk |
| ------------------------------ | :-------------------------------: | :-----------------: |
| Llega correo (ID 8815)         | 22:59:08.946                      | 23:00:29.594        |
| Firewall bloquea URL (ID 8816) | 23:00:22.946                      | 23:01:43.594        |
| Alertas creadas                | 23:01 (ID 8815) — 23:02 (ID 8816) | -                   |
| Triaje y reporte               | 23:04 (triaje) — 00:20 (reporte)  | -                   |

## Pasos para la remediación
1. **Bloqueo del correo**: Bloquear el correo electrónico asociado al correo de phishing.
2. **Corroborar con el usuario**: Asegurarse de que el usuario **H. Harris** no ha ingresado más información o ha utilizado su correo institucional en otras plataformas ajenas a la organización.
3. **Monitoreo del tráfico de red**: Crear reglas más estrictas para evitar correos de phishing y monitorear el tráfico saliente de la red.
4. **Capacitación en concientización de los usuarios**: Concientizar a los usuarios acerca del phishing con el fin de evitar hacer clic en enlaces de dudosa procedencia y evitar la exfiltración de credenciales.

## Lecciones aprendidas
1. Hay que dar una buena documentación sobre el(los) incidente(s) y relacionarlos como es debido
2. Correlacionar los logs en el SIEM es la parte más importante para el análisis de un reporte de seguridad
3. Los *playbooks* son una herramienta útil, si una alerta tiene uno se debe de utilizar, así será más fácil hacer el análisis de una alerta ya documentada
4. No entrar en desesperación. Hay que tener paciencia al analizar los logs y las alertas en el dashboard
5. Las queries en el SIEM son importantes para filtrar de forma más eficiente lo que se está buscando \
   ![](<./Images/8815_splunk_search.png>)
6. El veredicto es lo que dicta si es un **Verdadero** o **Falso positivo** y determina si se debe de escalar a un analista L2 con la documentación e investigaciones previamente hechos
7. Se deben de dar acciones correctivas para evitar un futuro incidente como el que se analizó
