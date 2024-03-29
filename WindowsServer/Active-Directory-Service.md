## Microsoft Active Directory Services

---

[Back to Windows-Servers](Default.md) - [Back to home](../README.md)
___

>Install Active Directory from Install-medium

*Read Only Domain Controller:*
```batch
ntdsutil
ntdsutil:activate instance ntds
ntdsutil:ifm
## IFM: create sysvol rodc D:\RODC
## Snapshot für RODC-Medien wird erstellt...
IFM:quit
ntdsutil:quit

## Folder structure:
D:\RODC
	|- Active Direcory
		|-ntds.dit
	|-SYSVOL
		|-htdom.local
			|-Policies
			|-scripts
```

*Domain Controller:*
```batch
ntdsutil
ntdsutil:activate instance ntds
ntdsutil:ifm
## IFM: create sysvol full D:\FullDC
## Snapshot wird erstellt...
IFM:quit
ntdsutil:quit

## Folder structure:
D:\FullDC
	|- Active Direcory
		|-ntds.dit
	|-SYSVOL
		|-htdom.local
			|-Policies
			|-scripts
	|-registry
		|-SECURITY
                |-SYSTEM
```
---

>find Active Directory schema version

```batch
reg query HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters

dsquery * CN=Schema,CN=Configuration,DC=htdom,DC=local -Scope Base -attr objectVersion
```
```powershell
Get-ADObject "CN=Schema,CN=Configuration,DC=htdom,DC=local" -Properties objectVersion | Select objectVersion | Format-Table -Autosize
```
---

>FMSO Rollen verschieben

*FSMO Rollen anzeigen lassen*
```batch
netdom query /domain:domain.com fsmo
```

*FSMO Rollen Online verschieben*
```batch
ntdsutil
ntdsutil: roles
fsmo maintenance: connections
server connections: connect to server neueDCServer.htdom.local
server connections: quit
fsmo maintenance:transfer schema master
fsmo maintenance:transfer domain naming master (Bis Version 2003)
fsmo maintenance:transfer naming master (Ab Version 2008)
fsmo maintenance:transfer pdc
fsmo maintenance:transfer rid master
fsmo maintenance:transfer infrastructure master
fsmo maintenance: quit
ntdsutil: quit
```

*FSMO Rollen Offline verschieben*
```batch
ntdsutil
ntdsutil: roles
fsmo maintenance: connections
server connections: connect to server neueDCServer.htdom.local
server connections: quit
fsmo maintenance: seize schema master
fsmo maintenance: seize domain naming master (Bis Version 2003)
fsmo maintenance: seize naming master
fsmo maintenance: seize pdc
fsmo maintenance: seize rid master
fsmo maintenance: seize infrastructure master
fsmo maintenance: quit
ntdsutil: quit
```
---

