# Secure Enterprise Network — Cisco Packet Tracer

**Training lab / Учебный стенд.** Bilingual technical report and original evidence are included in the [review archive](https://github.com/Raibek012/cybersecurity-portfolio/tree/main/projects/secure-enterprise-network) once the binary assets are uploaded.

## RU — Краткое описание
Сегментированная учебная сеть: VLAN 10 IT, VLAN 20 ACCOUNTING, VLAN 30 Servers, VLAN 40 SYSLOG, VLAN 99 MANAGEMENT. Router-on-a-Stick, DHCP, Extended ACL, SSH v2, Sticky MAC Port Security и централизованный Syslog.

| Проверка | Подтверждённый результат |
|---|---|
| 802.1Q trunk | Gi0/1: VLAN 10,20,30,40,99 allowed / active / forwarding |
| DHCP | Выданы 192.168.10.21 и 192.168.20.21 |
| ACL | VLAN 20: ICMP к IT/Management блокирован; к Server0 разрешён |
| SSH | Успешная интерактивная сессия PC0 → SW1 |
| Port Security Fa0/2 | Sticky MAC 00D0.584A.047E; тестовая замена устройства вызвала err-disabled; затем восстановлено |
| Syslog | Server1 получил три записи от SW1 (192.168.99.2) |

### IP plan
| VLAN | Gateway | Endpoint |
|---|---|---|
| 10 | 192.168.10.1 | PC0 (DHCP) |
| 20 | 192.168.20.1 | PC1 (DHCP) |
| 30 | 192.168.30.1 | Server0 192.168.30.10 |
| 40 | 192.168.40.1 | Syslog 192.168.40.10 |
| 99 | 192.168.99.1 | SW1 192.168.99.2 |

## EN — Summary
Authorized Cisco Packet Tracer training lab covering VLAN segmentation, Router-on-a-Stick, DHCP, inbound extended ACL, SSH management, Sticky MAC Port Security and centralized Syslog. Evidence confirms the SW1 Port Security violation and recovery, and three logs received by Server1 from SW1. Router0 Syslog receipt and a continuous Port-Security-to-Syslog correlation were **not** independently verified.

## Evidence files
The accompanying publication package contains `packet-tracer/topology.pkt`, 15 screenshots in `evidence/`, `README.en.md` and a bilingual `index.html`. Binary files are pending upload to this directory. Do not interpret missing files as verified GitHub attachments.

**Scope:** simulated course lab, not production incident response. Packet Tracer's displayed 1993 timestamps are simulated.
