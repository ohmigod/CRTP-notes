---
description: AD Files & Shares Enumeration
---

# Files & Shares

### Find shares on hosts in the current domain

**Note:** Only readable or writable shares, although not 100% sure.

{% code overflow="wrap" %}
```bash
#PowerView:
Invoke-ShareFinder -Verbose #List all shares.
Invoke-ShareFinder -Verbose -ExcludeStandard -ExcludeIPC -ExcludePrint #Exclude default shares.
```
{% endcode %}

### Find sensitive files on domain computers

```bash
#PowerView:
Invoke-FileFinder -Verbose
```

### Get all fileservers of the domain (high valuable targets)

**Note:** This find machines with the "fileserver" role installed. These servers are where lot of users authenticate to (DC's, fileserver, exchange, sharepoint...).

```bash
#PowerView:
Get-NetFileServer -Verbose
```
