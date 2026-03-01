
# 📄 PAGE 1 — Windows Server & Active Directory Foundations

## What is Active Directory (AD)

**EN:**
Active Directory is a directory service that acts as a central database for managing users, computers, and resources in a network. It provides authentication, authorization, and policy management.

**PL:**
Active Directory to usługa katalogowa działająca jako centralna baza danych do zarządzania użytkownikami, komputerami i zasobami w sieci. Zapewnia uwierzytelnianie, autoryzację i zarządzanie politykami.

---

# 🔥 Forest

**EN:**
A forest is the highest level in Active Directory. It contains one or more domains that trust each other and share the same schema and configuration. It acts as a security boundary and defines the overall structure of the environment.

**PL:**
Forest to najwyższy poziom w Active Directory. Zawiera jedną lub więcej domen, które sobie ufają i współdzielą schemat oraz konfigurację. Stanowi granicę bezpieczeństwa i określa ogólną strukturę środowiska.

---

# 🔥 Tree

**EN:**
A tree is a group of domains within the same forest that share a contiguous namespace. Domain names are connected, for example: `company.com` and `sales.company.com`. Domains in a tree automatically trust each other.

**PL:**
Tree to grupa domen w tym samym forest, które mają ciągłą przestrzeń nazw. Oznacza to, że nazwy domen są ze sobą powiązane, np. `company.com` i `sales.company.com`. Domeny w tree automatycznie sobie ufają.

---

# 🔥 Domain

**EN:**
A domain is a logical unit within a forest where users, computers, and resources are managed. It provides authentication and access control. Each domain has its own database and security policies.

**PL:**
Domena to logiczna jednostka w forest, w której zarządza się użytkownikami, komputerami i zasobami. Zapewnia uwierzytelnianie i kontrolę dostępu. Każda domena ma własną bazę danych i polityki bezpieczeństwa.

---

# 🔥 Organizational Unit (OU)

**EN:**
An OU is a container inside a domain used to organize users and computers. It enables applying Group Policies and delegating administrative control. OUs simplify management in large environments.

**PL:**
OU to kontener w domenie służący do organizowania użytkowników i komputerów. Umożliwia stosowanie polityk grupowych i delegowanie administracji. Ułatwia zarządzanie dużymi środowiskami.

---

# 🔥 Sites

**EN:**
A site is a group of networks with fast and reliable connectivity, usually representing a physical location such as an office or data center. Sites optimize authentication and replication traffic.

Active Directory uses Sites to:

* direct clients to the nearest Domain Controller
* control replication topology
* optimize network traffic

**PL:**
Site to grupa sieci z szybkim i stabilnym połączeniem, zwykle odpowiadająca fizycznej lokalizacji, np. biuru lub centrum danych. Sites optymalizują uwierzytelnianie i replikację.

Active Directory używa Sites, aby:

* kierować logowanie do najbliższego DC
* kontrolować replikację
* optymalizować ruch sieciowy

---

# 🔥 Schema

**EN:**
The schema defines which object types and attributes can exist in Active Directory. It ensures consistency across the entire forest. Any schema change affects the whole forest.

Example:
User object attributes include: First Name, Last Name, Email, Password.

**PL:**
Schemat określa, jakie typy obiektów i atrybuty mogą istnieć w Active Directory. Zapewnia spójność w całym forest. Każda zmiana schematu wpływa na cały forest.

---

# 🔥 Global Catalog (GC)

**EN:**
The Global Catalog is a distributed database containing searchable information about objects from all domains in the forest. It enables fast resource searches and supports logon, especially in multi-domain environments.

**PL:**
Global Catalog to rozproszona baza danych zawierająca informacje o obiektach ze wszystkich domen w lesie. Umożliwia szybkie wyszukiwanie zasobów i wspiera logowanie, szczególnie w środowiskach wielodomenowych.

---

# 🔥 Replication

**EN:**
Replication is the process of synchronizing Active Directory data between Domain Controllers to ensure consistency.

Intrasite replication is frequent and optimized for speed.
Intersite replication is compressed and scheduled to save bandwidth.

**PL:**
Replikacja to proces synchronizacji danych Active Directory między kontrolerami domeny, zapewniający spójność danych.

Replikacja wewnątrz site jest częsta i zoptymalizowana pod szybkość.
Replikacja między site jest kompresowana i planowana, aby oszczędzać pasmo.

---

# 🔥 Group Policy (GPO)

**EN:**
Group Policy allows administrators to centrally manage settings for users and computers, including security policies, passwords, and software configuration.

**PL:**
Group Policy umożliwia centralne zarządzanie ustawieniami użytkowników i komputerów, np. politykami bezpieczeństwa, hasłami czy konfiguracją oprogramowania.

---

## LSDOU — GPO Processing Order

Order:
**Local → Site → Domain → OU**

**EN:**
Policies are applied in LSDOU order, and the last applied policy has the highest priority.

**PL:**
Polityki stosowane są w kolejności LSDOU, a ostatnia ma najwyższy priorytet.

---

## Enforced vs Block Inheritance

**Enforced**

**EN:**
Ensures a GPO cannot be overridden by lower-level policies. Used for critical security settings.

**PL:**
Sprawia, że polityka nie może zostać nadpisana przez polityki niższego poziomu. Używane dla krytycznych ustawień bezpieczeństwa.

---

**Block Inheritance**

**EN:**
Prevents higher-level GPOs from applying to a container.

**PL:**
Blokuje stosowanie polityk z wyższych poziomów.

Example: specific OU excludes domain policies.

---

# 🔥 Delegation vs Group

## Delegation

**EN:**
Delegation is granting administrative permissions for specific tasks on Active Directory objects.

**PL:**
Delegacja to nadawanie uprawnień administracyjnych do wykonywania określonych zadań na obiektach Active Directory.

👉 What someone can do (CO może zrobić)

---

## Group

**EN:**
A group is a collection of users used to assign permissions efficiently.

**PL:**
Grupa to zbiór użytkowników używany do efektywnego przypisywania uprawnień.

👉 To whom permissions are assigned (KOMU)

---

**Example (Delegation):**

Helpdesk can:

* reset passwords
* unlock accounts

But cannot:

* modify GPO
* create DC
* be Domain Admin

---


