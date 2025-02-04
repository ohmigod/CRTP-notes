---
description: OAuth vulnerabilities
---

# OAuth

> OAuth is an open standard for access delegation, commonly used as a way for internet users to grant websites or applications access to their information on other websites but without giving them the passwords.

### URL Schema

Normal OAuth authentication starts with a URL like this:

{% code overflow="wrap" %}
```url
http://facebook.com/oauth/authorize?response_type=code&redirect_uri=http://client.oauth%2Fcallback&scope=view_gallery&client_id=1231231141
```
{% endcode %}

| Response\_type  | This is what we request (code or token)                        |
| --------------- | -------------------------------------------------------------- |
| Redirect\_uri   | Tells the Authentication service where to give the code/token  |
| scope           | Permissions that are being requested                           |
| client\_id      | The ID identifying that client                                 |

* "**client**" is the web that wants to pull data from facebook.
* "**code**" is used to later request a token. We can directly call for a token also.&#x20;

### Unvalidated redirect\_uri parameter

If the authorization server does not validate that the redirect URI belongs to the client, it is susceptible to two types of attacks.

* **Open Redirect.**
* **Account hijacking by stealing authorization codes:** If we redirect to a site under our control, we will receive the authorization code. Then, we may be able to exchange it for an access token and thus get access to the user's resources.

### Weak authorization codes

If the authorization codes are weak, we can try to guess/brute-force them at the token endpoint. This is useful when the client secret is compromised, not used, or not validated.
