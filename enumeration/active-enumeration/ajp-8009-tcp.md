---
description: Pentesting AJP (Apache JServ Protocol)
---

# AJP (8009/tcp)

> Apache JServ Protocol (AJP) is used for communication between Tomcat and Apache web server. This protocol is binary and is enabled by default. Anytime the web server is started, AJP protocol is started on port 8009. It is primarily used as a reverse proxy to communicate with application servers.

### AJP Proxy

AJP proxy might be a gateway to internal resources (admin panels or unpublished websites). We can configure our own instance of Apache to connect to a remote ajp port and then visit 127.0.0.1 to see whether it contains any interesting content.

{% code overflow="wrap" %}
```bash
#1. Install Apache on your system
apt-get install apache2

#2. Install ajp-related module and enable it
apt-get install libapache2-mod-jk
a2enmod proxy_ajp

#3. Create the following file
nano /etc/apache2/sites-enabled/ajp.conf

#4. Paste the following code
ProxyRequests Off
<Proxy *>
Order deny,allow
Deny from all
Allow from localhost
</Proxy>
ProxyPass / ajp://[TARGETIP]:8009/
ProxyPassReverse / ajp://[TARGETIP]:8009/

#5. Restart apache and we should be able to visit the remote website at http://127.0.0.1
```
{% endcode %}
