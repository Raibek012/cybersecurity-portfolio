# Incident Report 01 — Caldera / Sandcat on Windows (WS01 and WS02)

**SOC / DFIR / Threat Hunting · Training case · Events: 07 September 2026 · Report: 21 September 2026**

> This case study is based on the original 12-page incident report and its Wazuh screenshots. The screenshots, not a full raw-log export, are the evidence basis. Lab addresses are private test-network identifiers. The original report's authorization status was preliminary.

## 1. Investigation scope and evidence

| Field | Documented value |
|---|---|
| Hosts | WS01 (192.168.10.20; Wazuh agent 002), WS02 (192.168.10.21; agent 004) |
| Observed interval | 07.09.2026, 20:03–20:05 (UI time, assumed UTC+5) |
| Source | Wazuh `wazuh-alerts-4.x-2026.09.07`; process fields consistent with Sysmon Event ID 1 |
| Suspected C2 | `192.168.100.10:8888`, agent group `red` |
| Evidence | Three Wazuh event screenshots transcribed and correlated in the source report |

## 2. Observed event table

| Time (UI) | Host | Alert ID | Evidence / process |
|---|---|---|---|
| 20:04:21.131 | WS01 | `EyhmfKABDIqkImm0HrQy` | Encoded PowerShell, decoded as `wmic process get executablepath,name,processid,parentprocessid`; output redirected to `%APPDATA%\\vmtools.log` |
| 20:04:49.116 | WS02 | `LShmfKABDIqkImm0o7cf` | Encoded PowerShell; decoded `tasklist /m`; parent `C:\\Users\\Public\\dock.exe` |
| 20:04:50.038 | WS01 | ID cropped in screenshot | Encoded PowerShell; decoded `tasklist /m`; same parent process as first WS01 event |

The parent processes carried `-server http://192.168.100.10:8888 -group red`. The Windows processes ran under Administrator / High integrity. The decoded commands establish **process discovery**, not credential theft or exfiltration.

## 3. Analyst timeline

| Time (UTC+5, report's interpretation) | Finding | Basis |
|---|---|---|
| 26 Aug 09:47 | Administrator logon sessions on WS01/WS02 | Logon GUID interpretation; medium confidence |
| 27 Aug 09:54–09:57 | Parent agent processes appear | Parent ProcessGuid interpretation; medium confidence |
| 07 Sep 20:03:57 | WS01 process enumeration | ProcessGuid; high confidence |
| 07 Sep 20:04:48 | `tasklist /m` on both hosts within the same second | ProcessGuid; high confidence |
| 07 Sep 20:04:49–50 | Wazuh alerts recorded | Alert timestamps |

The report interpreted the GUID values as indicating the parent processes had been present for about eleven days. This is a reconstruction, not proof of continuous activity throughout that interval.

## 4. Detection and analysis workflow

1. Reviewed Wazuh event details for the two hosts and aligned alert timestamps.
2. Compared parent images, command lines, account and integrity fields.
3. Decoded the Base64 PowerShell commands as UTF-16LE and classified the observable behavior as process discovery.
4. Correlated ProcessGuid/LogonGuid and constructed a timeline with explicit confidence labels.
5. Documented gaps and proposed pivots for Sysmon EID 3/11, Windows Security events, PowerShell 4104 and Caldera server logs.

**MITRE ATT&CK (report mapping):** T1059.001 PowerShell, T1047 WMI, T1027.010 Command Obfuscation; discovery activity from process enumeration.

## 5. Result versus limits

| Supported by evidence | Not established |
|---|---|
| Caldera/Sandcat-associated parent command lines and encoded PowerShell | Whether the operation was formally authorized |
| Process discovery commands on both Windows hosts | Initial access method and complete prior activity |
| Shared C2 argument and synchronized discovery | Credential access, lateral movement or exfiltration |
| Wazuh screenshots and analyst timeline | Complete raw logs or confirmed network beacon telemetry |

**Assessment in the original report:** High preliminary severity; high confidence of Caldera/Sandcat identification. Confirmation from the lab owner is needed to classify the activity as an authorized exercise.

## 6. Original evidence

The supplied original report contains the full tables, decoded command details, IOC/IOA, hunting hypotheses, limitations, recommendations and Wazuh screenshot appendix. See the accompanying publication evidence package for the original report and images; do not represent a missing GitHub attachment as uploaded evidence.

**RU:** Исследованы три события Wazuh на WS01/WS02, разобраны закодированные команды PowerShell, построен таймлайн по GUID и отделены подтверждённые действия Discovery от неподтверждённых последствий.
