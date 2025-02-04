---
description: Security Assertion Markup Language Vulnerabilitie
---

# SAML

> Security Assertion Markup Language (SAML) is an open standard for exchanging authentication and authorization data between parties, in particular, between an identity provider and a service provider. SAML is an XML-based markup language for security assertions.

### SAML Workflow

<figure><img src="../../.gitbook/assets/saml.png" alt=""><figcaption><p><a href="https://kb.swivelsecure.com/w/index.php?title=File:Saml_workflow.gif&#x26;limit=50">SAML Workflow</a></p></figcaption></figure>

### SAML Attacks

* [ ] Interfere with SAML step 6 (response)
* [ ] Try changing the username and delete the signature (maintaining the tag empty)
* [ ] Try [XXE](../vulnerabilities/xxe.md)
* [ ] Try [injecting comments](https://developer.okta.com/blog/2018/02/27/a-breakdown-of-the-new-saml-authentication-bypass-vulnerability#cryptographic-signing-issues) which will be removed by the parser.

### Resources

{% embed url="https://www.economyofmechanism.com/github-saml#id10" %}

{% embed url="https://epi052.gitlab.io/notes-to-self/blog/2019-03-07-how-to-test-saml-a-methodology/" %}

{% embed url="https://epi052.gitlab.io/notes-to-self/blog/2019-03-13-how-to-test-saml-a-methodology-part-two/" %}
