# TryHackMe — SOC Simulator: Phishing de falso paquete de Amazon
**Dificultad** -> easy | **Date** -> 22-oct-2026 | **Type** -> Free

## Resumen

## ID 8815 — `Inbound email containing suspicious external link`

Un correo sospechoso de Amazon con una terminación extraña `.biz`. Los correos de Amazon no terminan así.

![8815\_amazon\_email\_impersonation](8815_amazon_email_impersonation.png)

La regla se dispara cuando llega un correo con uno o más links que van a sitios externos y potencialmente sospechosos.
De acuerdo con el playbook, hay que analizar el contenido de los emails. Al revisar el link con TryDetectThis se marca como malicioso

![8815\_link](8815_link.png)

#### Reporte del incidente

Para ubicar las marcas de tiempo, primero se utilizó la que marca dentro del dashboard y posteriormente en Splunk, la búsqueda se hizo mediante el correo: `h.harris@thetrydaily.thm` para únicamente revisar los logs relacionados a dicho correo.

![8815\_splunk\_search](8815_splunk_search.png)

```
Time of activity: 10/22/2026 22:59:08.946 (dashboard timestamp) 
10/22/2026 23:00:29.594 (splunk timestamp)

List of Affected Entities: h.harris@thetrydaily.thm; sender urgents@amazon[.]biz

Reason for Classifying as True Positive: the email sender impersonate an official Amazon link, generic greeting: "Dear Customer", creates urgency "48-hours..." and calls for action "Action required" and the url is shortened to hide its destination. TryDetectThis flags as a malicious

Reason for Escalating the Alert: phishing attack with a likely click attempt (blocked by the firewall). L2 must verify the host was not compromised and check whether other users received the email.

Recommended Remediation Actions: block the suspicious email at firewall. Delete/quarantine the email for the inbox. confirm with the user whether he clicked and entered any information 

List of Attack Indicators: sender: urgents@amazon[.]biz; subject: "Your Amazon Package Couldn't Be Delivered – Action Required";  URL: http://bit[.]ly/3sHkX3da12340
```
