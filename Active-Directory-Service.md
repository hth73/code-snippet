## Alle Rund um das Active Directory

[Windows-Server](https://github.com/HelmutThurnhofer/snippet/blob/master/Windows-Server.md) - [Home](https://github.com/HelmutThurnhofer/snippet/blob/master/README.md)
___

**Active Directory von einem Installations-Medium installieren**

*RODC Server:*
```batch
ntdsutil
ntdsutil:activate instance ntds
ntdsutil:ifm
## IFM: create sysvol rodc D:\RODC
## Snapshot für RODC-Medien wird erstellt...
IFM:quit
ntdsutil:quit

## Hier wird folgende Ordnerstruktur angelegt:
D:\RODC
	|- Active Direcory
		|-ntds.dit
	|-SYSVOL
		|-htdom.local
			|-Policies
			|-scripts
```

*DC Server:*
```batch
ntdsutil
ntdsutil:activate instance ntds
ntdsutil:ifm
## IFM: create sysvol full D:\FullDC
## Snapshot wird erstellt...
IFM:quit
ntdsutil:quit

## Hier wird folgende Ordnerstruktur angelegt:
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
___

**Active Directory Schema Version herausfinden**

```batch
reg query HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters

dsquery * CN=Schema,CN=Configuration,DC=htdom,DC=local -Scope Base -attr objectVersion
```
```powershell
Get-ADObject "CN=Schema,CN=Configuration,DC=htdom,DC=local" -Properties objectVersion | Select objectVersion | Format-Table -Autosize
```
___

**FMSO Rollen verschieben**

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
