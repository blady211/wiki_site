
# 📄 PAGE 5 — Windows Server & Active Directory PowerShell

# 💻 PowerShell in Active Directory Administration

**EN:**
PowerShell allows automation and bulk management of Active Directory objects such as users, groups, computers, and policies.

**PL:**
PowerShell umożliwia automatyzację i masowe zarządzanie obiektami Active Directory, takimi jak użytkownicy, grupy, komputery i polityki.

---

# 🔥 Active Directory Module

**EN:**
Most AD commands come from the ActiveDirectory PowerShell module.

Import module:

```powershell
Import-Module ActiveDirectory
```

Check availability:

```powershell
Get-Module -ListAvailable ActiveDirectory
```

**PL:**
Większość poleceń AD pochodzi z modułu ActiveDirectory.

---

# 👤 Users

## Get user

```powershell
Get-ADUser -Identity user
```

**EN:** Gets user details
**PL:** Pobiera informacje o użytkowniku

---

## List all users

```powershell
Get-ADUser -Filter *
```

---

## Create user

```powershell
New-ADUser -Name "John Doe" -SamAccountName jdoe -AccountPassword (Read-Host -AsSecureString) -Enabled $true
```

---

## Modify user

```powershell
Set-ADUser jdoe -EmailAddress john@company.com
```

---

## Delete user

```powershell
Remove-ADUser jdoe
```

---

## Reset password

```powershell
Set-ADAccountPassword jdoe -Reset
```

---

## Unlock account

```powershell
Unlock-ADAccount jdoe
```

---

## Find locked accounts

```powershell
Search-ADAccount -LockedOut
```

---

# 👥 Groups

## Get group

```powershell
Get-ADGroup "GroupName"
```

---

## Group members

```powershell
Get-ADGroupMember "GroupName"
```

---

## Add user to group

```powershell
Add-ADGroupMember "GroupName" -Members jdoe
```

---

## Remove user from group

```powershell
Remove-ADGroupMember "GroupName" -Members jdoe
```

---

# 🖥 Computers

## List computers

```powershell
Get-ADComputer -Filter *
```

---

## Disable computer

```powershell
Disable-ADAccount computer01
```

---

# 📜 Group Policy

## List all GPO

```powershell
Get-GPO -All
```

---

## Get specific GPO

```powershell
Get-GPO -Name "PolicyName"
```

---

## Create GPO

```powershell
New-GPO -Name "PolicyName"
```

---

## Resultant Set of Policy

```powershell
Get-GPResultantSetOfPolicy -ReportType Html -Path report.html
```

---

## Force GPO update

```powershell
Invoke-GPUpdate -Computer computer01
```

---

# 🌲 Domain & Forest

## Domain info

```powershell
Get-ADDomain
```

---

## Forest info

```powershell
Get-ADForest
```

---

## Domain Controllers

```powershell
Get-ADDomainController -Filter *
```

---

# 🔄 Replication

## Replication failures

```powershell
Get-ADReplicationFailure
```

---

# 🔐 Secure Channel

```powershell
Test-ComputerSecureChannel
```

**EN:** Verifies domain trust
**PL:** Sprawdza połączenie z domeną

---

# 🌐 DNS

```powershell
Resolve-DnsName domain.local
```

---

# ⚙️ Services & Processes

```powershell
Get-Service
Restart-Service DNS
Get-Process
Stop-Process -Id 1234
```

---

# 📊 Event Logs

```powershell
Get-EventLog -LogName Security
Get-WinEvent -LogName Security
```

---

# 🌐 Network

```powershell
Test-NetConnection server01
Get-NetIPAddress
Get-NetTCPConnection
```

---

# 🔥 Bulk Operations Example

## Create multiple users from CSV

```powershell
Import-Csv users.csv | ForEach-Object {
 New-ADUser -Name $_.Name `
            -SamAccountName $_.Sam `
            -AccountPassword (ConvertTo-SecureString $_.Password -AsPlainText -Force) `
            -Enabled $true
}
```

---

# 🔥 Find inactive users

```powershell
Search-ADAccount -AccountInactive -TimeSpan 90.00:00:00
```

---

# 🔥 Disable inactive users

```powershell
Search-ADAccount -AccountInactive -TimeSpan 90.00:00:00 | Disable-ADAccount
```

---

# 🎯 Interview Statement

**EN:**
“PowerShell allows me to automate user and group management, monitor AD health, and quickly troubleshoot replication or policy issues.”

**PL:**
„PowerShell pozwala mi automatyzować zarządzanie użytkownikami i grupami, monitorować stan AD oraz szybko diagnozować problemy replikacji i GPO.”

---

# ⭐ Core Commands to Remember

```
Get-ADUser
Add-ADGroupMember
Search-ADAccount -LockedOut
Get-ADDomainController
Get-ADForest
Test-ComputerSecureChannel
Get-GPO
Invoke-GPUpdate
Resolve-DnsName
Get-WinEvent
```

---

