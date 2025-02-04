---
description: Windows Defender Exclusion Paths
---

# Exclusion Paths

> Exclusion paths define exclusions for Microsoft Defender Antivirus that apply to scheduled scans, on-demand scans, and always-on, real-time protection and monitoring.&#x20;

### Get a list of current exclusion paths from Windows Defender

```bash
#Needs local admin privileges
Get-MpPreference | select-object -expandproperty exclusionpath
```

Here we can upload our malicious scripts (Mimikatz, powerview, powerup...) and the AV won't alert anything.

### Add an exclusion path

```bash
#Needs local admin privileges
Add-MpPreference -ExclusionPath "C:\XXX\XXX\"
```

### Remove an exclusion path&#x20;

```
#Needs local admin privileges
Remove-MpPreference -ExclusionPath "C:\XXX\XXX\"
```
