---
description: Subdomain Takeover
---

# Subdomain Takeover

> Sub-domain takeover vulnerability occur when a sub-domain (subdomain.example.com) is pointing to a service (e.g: GitHub, AWS/S3,..) that has been removed or deleted. This allows an attacker to set up a page on the service that was being used and point their page to that sub-domain.

### Example

For example, if subdomain.example.com was pointing to a GitHub page and the user decided to delete their GitHub page, an attacker can now create a GitHub page, add a CNAME file containing subdomain.example.com, and claim subdomain.example.com.

### Automation

Using [takeover](https://github.com/m4ll0k/takeover) we can quickly identify potential subdomain takeovers:

{% code overflow="wrap" %}
```bash
#https://github.com/m4ll0k/takeover
python3 takeover.py -l targets.txt -v -T 5 -k -t 5
```
{% endcode %}

### Verifying the vulnerability

Go to [can-i-take-over-xyz](https://github.com/EdOverflow/can-i-take-over-xyz) and search for the name of the service you are targeting in the issues tab. That way you can see the on-going discussion and more detailed steps on how to claim the subdomain takeover.&#x20;
