# 📄 PAGE 2 — Windows Server & Active Directory Security & Authentication

## 🔐 Authentication in Active Directory

**EN:**  
Active Directory uses authentication protocols and identity mechanisms to verify users and services securely.

**PL:**  
Active Directory używa protokołów uwierzytelniania i mechanizmów tożsamości do bezpiecznej weryfikacji użytkowników i usług.

---

## 🔥 Kerberos

**EN:**  
Kerberos is the default authentication protocol in Active Directory. It uses tickets instead of sending passwords over the network.

Authentication flow:
1. User receives **TGT**
2. DC issues **Service Ticket**
3. Service validates ticket

**PL:**  
Kerberos to domyślny protokół uwierzytelniania AD. Używa biletów zamiast przesyłania haseł.

---

## 🔥 NTLM

**EN:**  
NTLM is a legacy Microsoft authentication protocol used when Kerberos is not available.

Weaknesses:
- No mutual authentication
- Pass-the-Hash vulnerable
- NTLM relay
- Weak crypto

**PL:**  
NTLM to starszy protokół używany gdy Kerberos nie działa.

---

## 🔥 Kerberos Tickets

**EN:**  
Tickets are encrypted authentication tokens issued by DC.

Types:
- TGT
- Service Ticket

**PL:**  
Bilety Kerberos to zaszyfrowane tokeny uwierzytelniania.

---

## 🔥 SID (Security Identifier)

**EN:**  
SID is a unique security identity assigned to users, groups, and computers.

**PL:**  
SID to unikalny identyfikator bezpieczeństwa.

---

## 🔥 SPN (Service Principal Name)

**EN:**  
SPN uniquely identifies a service instance in AD for Kerberos authentication.

**PL:**  
SPN identyfikuje usługę w AD dla Kerberos.

---

## 🔥 Golden Ticket Attack

**EN:**  
Golden Ticket allows forging Kerberos TGT using KRBTGT hash.

Impact:
- Impersonate any user
- Domain Admin
- Persistent access

**PL:**  
Golden Ticket umożliwia generowanie fałszywego TGT.

---

## 🔥 Pass-the-Hash (PtH)

**EN:**  
Uses stolen NTLM hash for authentication without password.

Indicators:
- Logon Type 3
- NTLM
- Lateral movement

**PL:**  
Użycie skradzionego hasha NTLM do logowania.

---

## 🔥 Brute Force

**EN:**  
Repeated password attempts until success.

Indicators:
- Many 4625
- 4740 lockouts

**PL:**  
Wielokrotne próby logowania.

---

## 🔥 USN Rollback

**EN:**  
Occurs when DC is restored from old snapshot causing replication inconsistency.

**PL:**  
Występuje po przywróceniu DC z snapshotu.

---

## 🔥 Tombstone Lifetime

**EN:**  
Deleted AD objects are retained for 180 days before permanent removal.

Purpose:
- Deletion replication
- Prevent lingering objects

**PL:**  
Usunięte obiekty AD przechowywane są 180 dni.

---

## 🔥 Loopback Processing

**EN:**  
User GPO settings applied based on computer instead of user.

Modes:
- Replace
- Merge

**PL:**  
Ustawienia użytkownika zależne od komputera logowania.

---

## 🔐 Securing Privileged Accounts

**EN:**  
Best practices:
- Least privilege
- Separate admin accounts
- MFA
- Log monitoring
- Password rotation

**PL:**  
Najlepsze praktyki:
- Minimalne uprawnienia
- Oddzielne konta admin
- MFA
- Monitoring
- Rotacja haseł

---

## 🔐 Least Privilege Principle

**EN:**  
Users receive only minimum permissions required.

**PL:**  
Użytkownicy mają minimalne potrzebne uprawnienia.

---

## 🔎 Detecting Suspicious Logons

**EN:**  
Monitor events:
- 4624
- 4625
- 4648
- 4672

Indicators:
- Night logons
- New host
- Admin anomalies

