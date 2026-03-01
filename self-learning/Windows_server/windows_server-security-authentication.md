
# 📄 PAGE 2 — Windows Server & Active Directory Security & Authentication

# 🔐 Authentication in Active Directory

Active Directory uses authentication protocols and identity mechanisms to verify users and services in a secure way.

---

# 🔥 Kerberos

**EN:**
Kerberos is the default authentication protocol in Active Directory. It uses tickets instead of sending passwords over the network.

Authentication flow:

1. User logs in and receives a **TGT (Ticket Granting Ticket)**
2. When accessing a service, the Domain Controller issues a **Service Ticket**
3. The service validates the ticket and grants access

Passwords are not repeatedly transmitted.

**PL:**
Kerberos to domyślny protokół uwierzytelniania w Active Directory. Używa biletów zamiast przesyłania haseł przez sieć.

Przebieg:

1. Użytkownik otrzymuje **TGT (Ticket Granting Ticket)**
2. Przy dostępie do usługi DC wydaje **Service Ticket**
3. Usługa sprawdza bilet i przyznaje dostęp

Hasło nie jest przesyłane wielokrotnie.

---

# 🔥 NTLM

**EN:**
NTLM is an older Microsoft authentication protocol. It is used as a fallback when Kerberos cannot be used.

Security weaknesses:

* No mutual authentication
* Vulnerable to Pass-the-Hash
* Supports NTLM relay attacks
* Uses weaker cryptography

**PL:**
NTLM to starszy protokół uwierzytelniania Microsoft. Używany jako fallback, gdy Kerberos nie może zostać użyty.

Słabości:

* Brak wzajemnej autentykacji
* Podatność na Pass-the-Hash
* Możliwe NTLM relay
* Słabsza kryptografia

---

# 🔥 Kerberos Tickets

**EN:**
A ticket is an encrypted authentication token issued by the Domain Controller. Instead of sending a password to each service, the client presents a ticket. Tickets are time-limited.

Types:

* **TGT — Ticket Granting Ticket**
* **Service Ticket**

**PL:**
Bilet Kerberos to zaszyfrowany token uwierzytelniający wydawany przez kontroler domeny. Zamiast hasła klient przedstawia bilet. Bilety są czasowe.

Typy:

* **TGT**
* **Service Ticket**

---

# 🔥 SID (Security Identifier)

**EN:**
A SID is a unique security identity assigned to users, groups, and computers. Windows internally uses SIDs, not names. Permissions are assigned to SIDs.

**PL:**
SID to unikalny identyfikator bezpieczeństwa przypisywany użytkownikom, grupom i komputerom. Windows wewnętrznie używa SID, nie nazw. Uprawnienia są przypisywane do SID.

---

# 🔥 SPN (Service Principal Name)

**EN:**
SPN is a unique identifier for a service in Active Directory. It allows clients to locate and authenticate to the correct service using Kerberos.

**PL:**
SPN to unikalny identyfikator usługi w Active Directory. Umożliwia klientom odnalezienie i uwierzytelnienie się do właściwej usługi przy użyciu Kerberos.

---

# 🔥 Golden Ticket Attack

**EN:**
A Golden Ticket attack occurs when an attacker forges a Kerberos TGT using the KRBTGT account hash. This allows creating valid tickets for any user with any privileges.

Impact:

* Impersonate any user
* Domain Admin privileges
* Long-term persistence
* Full domain compromise

**PL:**
Golden Ticket to atak polegający na wygenerowaniu fałszywego TGT przy użyciu hasha konta KRBTGT. Umożliwia tworzenie ważnych biletów dla dowolnego użytkownika z dowolnymi uprawnieniami.

Efekt:

* Podszywanie się pod dowolnego użytkownika
* Uprawnienia Domain Admin
* Długotrwały dostęp
* Pełne przejęcie domeny

---

# 🔥 Pass-the-Hash (PtH)

**EN:**
In Pass-the-Hash, the attacker steals an NTLM hash from memory and uses it to authenticate without knowing the password.

Indicators:

* Logon Type 3
* NTLM authentication
* No interactive logon
* Lateral movement

**PL:**
Pass-the-Hash polega na użyciu skradzionego hasha NTLM do uwierzytelnienia bez znajomości hasła.

Wskaźniki:

* Logon Type 3
* NTLM
* Brak logowania interaktywnego
* Ruch boczny

---

# 🔥 Brute Force Attack

**EN:**
Repeated authentication attempts using multiple passwords until success.

Indicators:

* Many 4625 events
* Account lockouts (4740)
* Attempts from single host
* Multiple accounts targeted

**PL:**
Atak brute force polega na wielokrotnych próbach logowania z różnymi hasłami.

Wskaźniki:

* Wiele 4625
* Lockout 4740
* Próby z jednego hosta
* Wiele kont

---

# 🔥 USN Rollback

**EN:**
USN rollback occurs when a Domain Controller is restored to an earlier state (for example from a VM snapshot), causing replication inconsistencies.

Effects:

* Replication failure
* Data inconsistency
* Lingering objects

**PL:**
USN rollback występuje, gdy kontroler domeny zostanie przywrócony do wcześniejszego stanu (np. snapshot VM), powodując niespójność replikacji.

Efekty:

* Błędy replikacji
* Niespójne dane
* Zalegające obiekty

---

# 🔥 Tombstone Lifetime

**EN:**
Tombstone Lifetime is the period during which deleted Active Directory objects are retained before permanent removal. Default is 180 days.

Purpose:

* Allow deletion replication
* Prevent lingering objects

**PL:**
Tombstone Lifetime to okres przechowywania usuniętych obiektów AD przed trwałym usunięciem. Domyślnie 180 dni.

Cel:

* Replikacja usunięcia
* Zapobieganie zalegającym obiektom

---

# 🔥 Loopback Processing

**EN:**
Loopback processing is a Group Policy mode where user settings are applied based on the computer the user logs onto rather than the user account.

Modes:

* **Replace** — only computer policies apply
* **Merge** — user + computer policies (computer wins conflicts)

**PL:**
Loopback processing to tryb GPO, w którym ustawienia użytkownika są stosowane na podstawie komputera logowania, a nie konta użytkownika.

Tryby:

* **Replace** — tylko polityki komputera
* **Merge** — użytkownik + komputer (komputer ma priorytet)

---

# 🔥 Securing Privileged Accounts

**EN:**
Best practices for securing privileged AD accounts:

* Minimize number of privileged accounts
* Least privilege principle
* Separate admin accounts
* MFA
* Monitor logs
* Password rotation

**PL:**
Najlepsze praktyki zabezpieczania kont uprzywilejowanych:

* Minimalna liczba kont
* Zasada najmniejszych uprawnień
* Oddzielne konta admin
* MFA
* Monitorowanie logów
* Rotacja haseł

---

# 🔥 Least Privilege Principle

**EN:**
Users and systems receive only the minimum permissions necessary to perform their tasks.

**PL:**
Użytkownicy i systemy otrzymują tylko minimalne uprawnienia potrzebne do wykonania zadań.

---

# 🔥 Detecting Suspicious Logons

**EN:**
Monitor authentication events and anomalies:

* 4624 — successful logon
* 4625 — failed logon
* 4648 — explicit credentials
* 4672 — privileged logon

Indicators:

* Night logons
* New host
* Admin logon anomalies
* SIEM alerts

**PL:**
Monitorowanie logowań i anomalii:

* 4624 — udane
* 4625 — nieudane
* 4648 — poświadczenia
* 4672 — admin

Wskaźniki:

* Logowania nocne
* Nowy host
* Nietypowe admin logony
* Alerty SIEM

---


