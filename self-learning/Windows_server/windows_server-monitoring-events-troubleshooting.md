
# 📄 PAGE 4 — Windows Server & Active Directory Monitoring, Events, SIEM & Troubleshooting

# 📊 Active Directory Monitoring

**EN:**
Active Directory monitoring ensures authentication, replication, and privileged activity operate securely and reliably.

**PL:**
Monitoring Active Directory zapewnia poprawne i bezpieczne działanie uwierzytelniania, replikacji i aktywności uprzywilejowanej.

---

# 🔐 Key Authentication Events

| Event ID | Meaning                         |
| -------- | ------------------------------- |
| 4624     | Successful logon                |
| 4625     | Failed logon                    |
| 4648     | Logon with explicit credentials |
| 4672     | Privileged logon                |
| 4768     | Kerberos TGT issued             |
| 4769     | Kerberos service ticket         |
| 4771     | Kerberos pre-auth failed        |
| 4776     | NTLM authentication             |

---

# 👤 Account Management Events

| Event ID | Meaning        |
| -------- | -------------- |
| 4720     | User created   |
| 4722     | User enabled   |
| 4725     | User disabled  |
| 4726     | User deleted   |
| 4740     | Account locked |

---

# 👥 Privileged Group Changes

| Event ID | Meaning                  |
| -------- | ------------------------ |
| 4728     | Added to global group    |
| 4732     | Added to local group     |
| 4756     | Added to universal group |

Critical groups to monitor:

* Domain Admins
* Enterprise Admins
* Administrators

---

# 🔎 Detecting Suspicious Activity

**EN:**
Indicators of compromise in AD logs:

* Night logons
* Logon from unusual host
* Privileged logon anomalies
* Multiple failed logons
* Lateral movement patterns

**PL:**
Wskaźniki podejrzanej aktywności:

* Logowania nocne
* Nietypowy host
* Anomalie logowań admin
* Wiele błędnych logowań
* Ruch boczny

---

# 🔐 Kerberos Attack Indicators

**Golden Ticket:**

* 4768 / 4769 anomalies
* Long ticket lifetime
* No AS-REQ flow
* Unexpected SID or groups

**PL:**
Wskaźniki Golden Ticket:

* Anomalie 4768 / 4769
* Nienaturalny czas biletu
* Brak normalnego AS-REQ
* Nietypowy SID

---

# 🔐 Pass-the-Hash Indicators

* Logon Type 3
* NTLM authentication
* No interactive logon
* Lateral movement

---

# 🔐 Brute Force Indicators

* Many 4625 events
* Account lockouts (4740)
* Attempts from single IP
* Multiple accounts targeted

---

# 📡 Active Directory Ports

| Port        | Protocol    | Purpose           |
| ----------- | ----------- | ----------------- |
| 88          | Kerberos    | Authentication    |
| 389         | LDAP        | Directory queries |
| 636         | LDAPS       | Secure LDAP       |
| 445         | SMB         | SYSVOL / Netlogon |
| 53          | DNS         | DC discovery      |
| 135         | RPC         | Services          |
| 49152-65535 | RPC Dynamic | Replication       |
| 3389        | RDP         | Remote admin      |

---

# 🧱 SIEM Architecture for AD Logs

**EN:**
Domain Controller logs are forwarded to a central collector and then to SIEM for correlation and alerting.

Flow:

1. Log sources (DC, servers, endpoints)
2. Log collection (WEF or agent)
3. Collector
4. SIEM

**PL:**
Logi DC są przesyłane do kolektora, a następnie do SIEM do analizy.

Przepływ:

1. Źródła logów
2. Zbieranie (WEF/agent)
3. Kolektor
4. SIEM

---

# 🧪 Daily Domain Controller Monitoring

## Authentication & Logons

Monitor:

* 4625 failures
* 4672 privileged logons
* 4768 / 4769 Kerberos

---

## Privileged Group Changes

## Monitor:

| Event | Meaning                   |
| ----- | ------------------------- |
| 4728  | Added to privileged group |
| 4732  | Added to local admin      |
| 4756  | Added to universal admin  |

---

## Replication Health

```bash
repadmin /replsummary
repadmin /showrepl
```

---

## SYSVOL & NETLOGON

```bash
net share
```

---

## DNS Health

```bash
dcdiag /test:dns
```

---

## Time Sync

```bash
w32tm /query /status
```

---

## Disk & Backup

```powershell
Get-PSDrive C
```

Verify backups exist.

---

# 🛠 AD Troubleshooting — GPO Not Applied

Steps:

1️⃣ Force refresh

```bash
gpupdate /force
```

2️⃣ Check applied policies

```bash
gpresult /r
```

3️⃣ Review RSOP

```bash
rsop.msc
```

4️⃣ Verify OU linkage & inheritance
5️⃣ Check Security Filtering & WMI filter
6️⃣ Review GroupPolicy logs

**Interview answer:**

**EN:**
“I start with gpresult and RSOP to confirm whether the GPO is applied. Then I verify OU linkage, filtering, DNS, and review GroupPolicy logs.”

---

# 🌐 DNS Failure Impact in AD

**EN:**
If DNS fails:

* Clients cannot locate DC
* Kerberos fails
* GPO fails
* Replication may break
* Domain apps fail

Because AD relies on DNS SRV records.

**PL:**
Gdy DNS nie działa:

* Klienci nie znajdują DC
* Kerberos nie działa
* GPO nie działa
* Replikacja się psuje
* Aplikacje domenowe przestają działać

---

# 🛠 Common AD Troubleshooting Commands

## Network

```bash
ipconfig /all
ping <host>
nslookup <domain>
tracert <host>
netstat -an
```

---

## Active Directory

```bash
whoami
whoami /groups
net user <user> /domain
net group "<group>" /domain
```

---

## Domain Controller Health

```bash
dcdiag
repadmin /replsummary
repadmin /showrepl
netdom query fsmo
```

---

## Group Policy

```bash
gpupdate /force
gpresult /r
```

---

## Event Logs

```bash
eventvwr.msc
```

---

To jest **Page 4 — Monitoring & Troubleshooting** gotowe do Twojej wiki.

---

