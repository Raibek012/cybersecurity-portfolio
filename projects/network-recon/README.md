# Network reconnaissance and Arkime correlation — Red/Blue training lab

**KAI SOC Academy · Interim report · 03–04 October 2026**

> This is an authorized educational exercise documented in the original three-page interim report. The analyst performed lab-side reconnaissance and access tests, then reviewed the resulting network evidence in Arkime. **No successful compromise was recorded.** A later Elastic/Kibana and Velociraptor final investigation was planned, not included in this report.

## 1. Lab and methodology

| Role | Documented system |
|---|---|
| Test target | `ms3-win11` / 192.168.10.50 |
| Testing host | Kali Linux VM over WireGuard, lab VPN interface `wg0` (10.252.1.134) |
| Defensive visibility | Arkime captured HTTP/WinRM traffic |
| Future analysis listed in source | Elastic/Kibana `logs-*` and Velociraptor; not completed in this interim report |

The documented Nmap phase used `-Pn -sV` service/version discovery. The report lists `-p-` and `-sC` as **next steps**, so this case does not claim those flags were used in the documented run.

## 2. Reconnaissance — documented service inventory

| Port | Service | Observation |
|---|---|---|
| 21/tcp | Microsoft FTP | Anonymous login succeeded; directory listing did not complete due to reported VM/NAT data-channel limitation |
| 80/tcp | Microsoft HTTPAPI 2.0 | Web service identified |
| 445/tcp | SMB | Anonymous/null session denied (`NT_STATUS_ACCESS_DENIED`) |
| 3389/tcp | RDP | Identified; not tested at this stage |
| 5985/tcp | WinRM | Authentication tests were documented |
| 8080/tcp | Apache Tomcat | `GET /` returned HTTP 200 |
| 8081/tcp | Jetty 12.1.13 | Identified; not further investigated |

## 3. Red-team activity recorded in the report

- Checked anonymous FTP access and attempted directory listing; login succeeded, listing failed.
- Checked SMB anonymous share enumeration; access was denied.
- Conducted an authorized training-lab WinRM credential-guessing exercise. The report records failed attempts and HTTP 401 responses, **not a successful login**.
- Mapped the observed authentication-guessing activity to MITRE ATT&CK T1110, with T1110.001 / T1110.003 considered in the source report.

## 4. Blue-team evidence — Arkime

The report correlated source/destination, HTTP method, response and client fingerprint in network sessions:

| Time (source report) | Flow | Arkime observation |
|---|---|---|
| 04 Oct 01:19:59 | Kali VPN → target :5985 | HTTP 401, POST /wsman, username `soc-analyst-5`, Python PSRP Client user-agent |
| 04 Oct 01:19:59 | Kali VPN → target :5985 | HTTP 401, POST /wsman, username `soc-analyst` |
| 04 Oct 01:07:17 | Kali VPN → target :8080 | HTTP 200, GET / from Tomcat |

**Analyst correlation:** the WinRM access attempts from the testing VM produced HTTP authentication failures visible in Arkime. An HTTP 200 from Tomcat means a web request succeeded; it is not evidence of host compromise.

## 5. Results and limitations

| Confirmed | Not confirmed / outside this interim report |
|---|---|
| Service inventory and reachable web endpoints | Successful compromise of the Windows target |
| FTP anonymous authentication (without listing) | FTP directory contents |
| SMB null-session denial | Anonymous SMB access |
| WinRM HTTP 401 traces in Arkime | Valid WinRM credentials or authenticated session |
| Kali-originated lab traffic correlated in Arkime | Final Elastic/Kibana investigation or Velociraptor findings |

**RU — роль атакующего и защитника:** Сначала на разрешённом стенде с Kali Linux проводилась разведка сервисов и проверка ограничений доступа. Затем как SOC-аналитик я сопоставил результаты с HTTP-сессиями в Arkime: POST /wsman и ответы 401, а также отдельный GET / с ответом 200. Успешная компрометация не зафиксирована.

Source: the supplied three-page **«Промежуточный отчёт по разведке и сетевому доступу»**, 03–04.10.2026. The publication evidence package preserves the source report. No screenshots were embedded in that supplied DOCX.
