---
description: AD OU Enumeration
---

# OU Enumeration

### Get OUs in the current domain

```bash
#PowerView:
Get-NetOU #List of oUs
Get-NetOU -FullData #Full data of the OUs

#AD Module:
Get-ADOrganizationalUnit -Filter * -Properties *
```

### Get GPOs applied to an OU

```bash
#PowerView:
Get-NetGPO -GPOName "{$GPLINK}" #Get the GPLINK from the Get-NetOU -FullData

#AD Module:
Get-GPO -Guid $GPLINK
```
