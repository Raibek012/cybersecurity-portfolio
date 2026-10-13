# Caldera / Sandcat — Windows investigation

**Type:** Training-lab case study (SOC / DFIR)  
**Tools:** Wazuh, Windows process telemetry (fields consistent with Sysmon Event ID 1), PowerShell analysis.

## Objective
Analyze suspicious process creation on two training Windows workstations and reconstruct the observable sequence.

## Observations
- Wazuh screenshots showed encoded PowerShell launched under Sandcat-associated parent processes on two hosts.
- Decoded commands related to process discovery, including process enumeration and module listing.
- The report correlated process-parent fields and timestamps to build a timeline.

## Assessment and limitations
The report assessed the Sandcat/Caldera identification with high confidence. Whether the activity was an authorized exercise could not be established from logs alone. Source evidence consisted of screenshots rather than a complete raw-log export. No credential theft, lateral movement or exfiltration was established by the supplied evidence.

## Analyst takeaway
Distinguish an observed suspicious tool and discovery behavior from unobserved downstream impact. Verify exercise authorization with the lab owner.

## Русский
Учебное расследование активности Caldera/Sandcat на двух Windows-хостах: анализ кодированного PowerShell, связей родительских и дочерних процессов и таймлайна. Авторизация активности и дальнейшее воздействие не подтверждались имеющимися скриншотами.

*Public summary only. Original logs, internal addresses, identifiers and screenshots are intentionally excluded.*
