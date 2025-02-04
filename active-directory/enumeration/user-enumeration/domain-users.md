---
description: AD Domain Users Enumeration
---

# Domain Users

### Get a list of all the domain users

```bash
#PowerView:
Get-NetUser #All users
Get-NetUser -Username $USER #Specific user
Get-NetUser -Domain $DOMAIN #All users from another domain

#AD Module:
Get-ADUser -Filter * -Properties * #All users
Get-ADUser -Identitiy $USER -Properties * #Specific user
```

### Get properties of domain users

{% code overflow="wrap" %}
```bash
#PowerView:
Get-UserProperty #Return full list of properties to query
Get-UserProperty -Properties $PROPERTY #Choose the property from the above command output

#Examples:
Get-UserProperty -Properties description #Description of all users (may contain passwords)
Get-UserProperty -Properties pwdlastset #Password last set (0 might indicate decoy users)
Get-UserProperty -Properties badpwcount #Bad password count (0 might indicate decoy users)
Get-UserProperty -Properties logoncount #Login count (0 might indicate decoy users)

#AD Module:
Get-ADUser -Filter * -Properties * | select -First 1 | Get-Member -MemberType *Property | select Name
Get-ADUser -Filter * -Properties * | select name,@{expression {{dametime]::fromFileTime($_.pwdlastset)}}
```
{% endcode %}

### Find a specific string in a user's property

{% code overflow="wrap" %}
```bash
#PowerView:
Find-UserField -SearchField Description -SearchTerm "*pass*"
Find-UserField -SearchField Description -SearchTerm "Jefe de*"

#AD Module:
Get-ADUser -Filter 'Description -like "*pass*"' - Properties Description | select name,Description
```
{% endcode %}
