---
description: Simple Object Access Protocol (SOAP) Vulnerabilities
---

# SOAP

> SOAP is the Simple Object Access Protocol, a messaging standard defined by the World Wide Web Consortium and its member editors. SOAP uses an XML data format to declare its request and response messages, relying on XML Schema and other technologies to enforce the structure of its payloads.

#### SOAP Definition

Try to find the API definition and pare it using wsdler. Then check every endpoint for potential vulnerabilities (SQLi, IDOR...)

```
/?wsdl
/xx.asmx?wsdl
```

#### SOAP Based XXE

![](../../.gitbook/assets/GetImage.png)

#### SOAP Based XSS

![](<../../.gitbook/assets/GetImage (1).png>)
