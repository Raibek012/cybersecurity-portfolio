# Sandcat delivery attempt — Linux

**Type:** Training-lab case study (SOC / DFIR)  
**Tools:** Wazuh, Linux auditd / EXECVE.

## Objective
Review Linux process telemetry for indications of Sandcat agent delivery.

## Observations
- Four curl executions with Sandcat-related request headers were recorded in the supplied report.
- The first two had effective UID of a regular user; the later two ran with effective UID 0.
- The login UID remained consistent, supporting correlation of the attempts.

## Assessment and limitations
The evidence supports **attempted delivery**, not confirmed successful download, execution or an established C2 channel. Later command arguments, execution events and network telemetry were absent from the screenshots. The Linux server destination was not independently visible.

## Analyst takeaway
Keep observation and inference separate, particularly when process execution and network evidence are missing.

## Русский
В учебном отчёте разобраны четыре события curl в Wazuh/auditd и смена эффективного UID на root. Попытка доставки наблюдалась; успешная загрузка, запуск файла и C2-соединение не были подтверждены.

*Sanitized public summary; no original addresses, usernames or raw evidence.*
