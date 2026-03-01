---

# 📄 PAGE 3 — Windows Server & Active Directory FSMO, Replication, SYSVOL, DC Health & Backup

# 🔧 FSMO Roles (Flexible Single Master Operations)

**EN:**
FSMO roles are specialized Active Directory roles responsible for specific operations that must be handled by a single Domain Controller.

**PL:**
Role FSMO to specjalne role Active Directory odpowiedzialne za operacje wykonywane tylko przez jeden kontroler domeny.

There are five FSMO roles:

* Schema Master
* Domain Naming Master
* PDC Emulator
* RID Master
* Infrastructure Master

---

## 🔹 Schema Master

**EN:**
Controls all schema changes in the forest. Only this role can modify object classes and attributes.

**PL:**
Kontroluje zmiany schematu w całym forest. Tylko ta rola może modyfikować klasy i atrybuty obiektów.

---

## 🔹 Domain Naming Master

**EN:**
Manages the domain namespace in the forest. Required for adding or removing domains.

**PL:**
Zarządza przestrzenią nazw domen w forest. Wymagana przy dodawaniu lub usuwaniu domen.

---

## 🔹 PDC Emulator

**EN:**
Most critical FSMO role in a domain.

Responsibilities:

* Time synchronization
* Password change propagation
* Account lockout handling
* Legacy compatibility
* GPO source

**PL:**
Najważniejsza rola FSMO w domenie.

Zadania:

* Synchronizacja czasu
* Propagacja zmian haseł
* Obsługa blokad kont
* Kompatybilność legacy
* Źródło GPO

---

## 🔹 RID Master

**EN:**
Allocates RID pools to Domain Controllers to ensure unique SIDs for new objects.

**PL:**
Przydziela pule RID kontrolerom domeny, zapewniając unikalne SID dla nowych obiektów.

---

## 🔹 Infrastructure Master

**EN:**
Maintains references to objects in other domains.

**PL:**
Aktualizuje referencje do obiektów z innych domen.

Note:
If all DCs are Global Catalogs, impact is minimal.

---

# 🔥 FSMO Failure Impact

| FSMO Role             | Failure Impact            |
| --------------------- | ------------------------- |
| Schema Master         | Cannot modify schema      |
| Domain Naming Master  | Cannot add/remove domains |
| PDC Emulator          | Logon & password issues   |
| RID Master            | Cannot create new objects |
| Infrastructure Master | Cross-domain group issues |

---

# 🔄 Active Directory Replication

**EN:**
Replication synchronizes AD data between Domain Controllers to maintain consistency.

**PL:**
Replikacja synchronizuje dane AD między kontrolerami domeny, zapewniając spójność.

Types:

* Intrasite — frequent, fast
* Intersite — scheduled, compressed

---

# 🔥 SYSVOL

**EN:**
SYSVOL is a shared folder on Domain Controllers containing Group Policy files and scripts. It must be identical on all DCs.

Contents:

* GPO files
* Logon scripts
* Startup scripts

**PL:**
SYSVOL to udział na kontrolerach domeny zawierający pliki GPO i skrypty. Musi być identyczny na wszystkich DC.

Zawiera:

* pliki GPO
* skrypty logowania
* skrypty startowe

If SYSVOL replication fails:

* Inconsistent policies
* Logon script failures
* GPO not applied

---

# 🔐 NETLOGON

**EN:**
NETLOGON is a shared folder containing authentication scripts and domain logon data. It is replicated between DCs.

**PL:**
NETLOGON to udział zawierający skrypty logowania i dane uwierzytelniania domeny. Jest replikowany między DC.

---

# 🩺 Domain Controller Health Checks

Goal: ensure authentication, replication, and services are healthy.

---

## Replication Health

**Commands:**

```bash
repadmin /replsummary
repadmin /showrepl
```

Check:

* Failures
* Last replication
* Partner errors

---

## Core Services

Must be running:

* Active Directory Domain Services
* DNS Server
* Netlogon

---

## SYSVOL & NETLOGON Shares

```bash
net share
```

Verify shares exist.

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

## Disk Space

```powershell
Get-PSDrive C
```

---

## Event Logs

Check:

* Directory Service
* DNS Server
* System
* Security

Example:

```powershell
Get-EventLog -LogName "Directory Service" -Newest 20 -EntryType Error
```

---

# 💾 Active Directory Backup

## System State Backup

**EN:**
System State backup includes AD database, SYSVOL, registry, and boot files.

Tools:

* Windows Server Backup
* wbadmin

**PL:**
Backup System State zawiera bazę AD, SYSVOL, rejestr i pliki startowe.

---

## Full Server Backup

Used for full Domain Controller recovery.

Tools:

* Veeam
* Commvault

---

## Snapshot (Virtual DC)

Point-in-time restore for virtualized DCs.

Note:
Improper restore may cause USN rollback.

---

## AD Recycle Bin

Logical recovery of deleted objects.
Not a backup.

---

# 🔄 Restore Types

## Non-Authoritative Restore

**EN:**
Restores a Domain Controller and then updates data from replication partners.

**PL:**
Odtwarza kontroler domeny, a następnie synchronizuje dane z innych DC.

---

## Authoritative Restore

**EN:**
Marks restored objects as authoritative so they overwrite replicated versions.

**PL:**
Oznacza przywrócone obiekty jako nadrzędne, nadpisując replikację.

---

# 🔧 Patching Domain Controllers

Best practice:

* Patch one DC at a time
* Verify AD health before patch
* Validate replication after reboot

Commands:

```bash
dcdiag
repadmin /replsummary
```

Interview answer:

**EN:**
“I patch Domain Controllers sequentially. Before patching I verify AD health using dcdiag and repadmin. After reboot I confirm replication and authentication before proceeding.”

---

