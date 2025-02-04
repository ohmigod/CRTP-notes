---
description: Pentesting SMB & NetBIOS
---

# SMB & NetBIOS (137-139/tcp,445/tcp)

> SMB (Server Message Block) is a network communication protocol for providing shared access to files, printers, and serial ports between nodes on a network.

> NetBIOS (Network Basic Input/Output System) is a program that allows applications on different computers to communicate within a local area network (LAN).

* Port 139: SMB originally ran on top of NetBIOS using port 139. NetBIOS is an older transport layer that allows Windows computers to talk to each other on the same network.
* Port 445: Later versions of SMB (after Windows 2000) began to use port 445 on top of a TCP stack. Using TCP allows SMB to work over the internet.

#### SMB Anonymous login

```bash
#Using smbclient:
smbclient -N -L \\\\$IP_ADDRESS\\
#-N: Don't ask for password
#-L: Get a list of shares available on a host

#Enter in valid found shares:
smbclient \\\\$IP_ADDRESS\\

#Using smbmap :
smbmap -H $IPADDRESS
smbmap -u $INVENTEDUSERNAME -H $IPADDRESS

#Mount valid shares (Windows):
mount -t cifs //$IP_ADDRESS/$SHARE /mnt/$CREATESHARE
#Change cifs for smbfs in linux?
```

#### SMB Using valid credentials

```bash
smbclient --user=$USER -L \\\\$IP_ADDRESS\\ #Using smbclient

smbclient -U '$USERNAME%$PASSWORD' \\\\$IPADDRESS\\$SHARE

smbmap -u $USER -p $PASSWORD -H $IP_ADDRESS #Using smbmap
```

#### Query the NetBIOS name service for valid NetBIOS names

```bash
#IPADDRESS can be a range (192.168.1.0/24)
nbtscan -r $IPADDRESS
```

#### Nmap NSE scripts

There are many interesting NSE scripts, take a look at:

{% code overflow="wrap" %}
```bash
ls -1 /usr/share/nmap/scripts/smb*
#Example:
nmap -v -p 139,445 --script=smb-os-discovery $IPADDRESS

sudo nmap -n -v -sV -Pn -p 445 --script=smb-ls,smb-mbenum,smb-enum-shares,smb-enum-users,smb-os-discovery,smb-security-mode,smb-vuln* $IPADDRESS
```
{% endcode %}

#### [enum4linux-ng](https://github.com/cddmp/enum4linux-ng)

> Enum4linux is a tool for enumerating information from Windows and Samba systems.

```bash
#Look for Null Session enumeration against the domain controller
enum4linux-ng -A DC1.acme.local
```

#### SMB Version

In order to grab the SMB Version we can use the following method:

```bash
#Execute the following command in one terminal:
sudo ngrep -i -d tun0 's.?a.?m.?b.?a.*[[:digit:]]'

#On another terminal:
smbclient -L $IPADDRESS -U "" -N
```

#### SMB to RCE

If we have READ/WRITE access to ADMIN$ or C$ with a valid user we can use psexec.py to obtain a shell:

```bash
python psexec.py $username@$IP_ADDRESS
Password:
```
