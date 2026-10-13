# Network reconnaissance and Arkime correlation

**Type:** Supervised training-lab interim report  
**Tools:** Nmap, Arkime, network-service analysis.

## Objective
Inventory services in an authorized lab and correlate access-control observations with network traffic.

## Observations
- The report documented a service inventory including FTP, SMB, WinRM and web services.
- Anonymous FTP authentication was observed, but directory listing was unsuccessful due to a reported data-channel/network limitation.
- Anonymous SMB access was denied.
- Arkime showed HTTP authentication failures corresponding to the documented lab access tests.

## Assessment and limitations
The report explicitly recorded **no successful compromise**. This was an interim reconnaissance report. Planned Elastic/Kibana and Velociraptor investigation was not included in the submitted material.

## Analyst takeaway
Record both positive and negative findings and correlate application responses with network evidence without overstating the result.

## Русский
Промежуточная работа по разрешённому учебному стенду: инвентаризация сервисов, проверка ограничений доступа и сопоставление сетевых ответов в Arkime. Успешная компрометация не зафиксирована.

*Sanitized summary; credential guesses and target addresses are excluded.*
