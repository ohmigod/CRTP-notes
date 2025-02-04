---
description: Hash length extension attack
---

# Hash length extension

> A length extension attack is a type of attack where an attacker can use Hash(_message1_) and the length of _message1_ to calculate Hash(_message1_ ‖ _message2_) for an attacker-controlled _message2_, without needing to know the content of _message1._

### Fundamentals

There are **web applications** that prepend a secret value to data, hash this value with a flawed alogithm and **provides the user with both the data and the resulting hash, but not the secret.** The vulnerability resides on that the server relies on the secret for data validation purposes.

**An attacker that doesn't know the value of the secret can still generate a valid hash** for (_secret || data || attacker\_controlled\_data_). This provides the ability to calculate a valid hash for a message **without knowing the value of the secret, only its length**.

<figure><img src="../../../.gitbook/assets/hlea.png" alt=""><figcaption><p><a href="https://www.roguesecurity.in/2017/05/14/length-extension-attack-and-how-it-can-be-exploited/">Hash Length Extension Attack</a></p></figcaption></figure>

In summary, **if we have the original data, the hash and the hash type**, we can bypass the MAC validation. Note that we should know or guess (brute-force) the length of the key. HMAC prevents this type of attack.

### Vulnerable algorithms

MD4, MD5, RIPEMD-160, SHA-0, SHA-1, SHA-256, SHA512, and WHIRLPOOL.

### Tool - hash\_extender

hash\_extender syntax:

{% code overflow="wrap" %}
```
./hash_exteder --data <original_data> -s <original_hash> --append <new_data> -f <hash_format> -l <length_secret> {--secret-min –-secret-max if we dont know the secret} --out-data-format=html --table 
```
{% endcode %}

hash\_extender example (brute-forcing key length):

{% code overflow="wrap" %}
```
./hash_exteder --data 'file.php' -s 'dd03bd22...' --append '../../etc/passwd' -f sha1 –-secret-min=10 –-secret-max=40 --out-data-format=html --table 
```
{% endcode %}

{% embed url="https://github.com/iagox86/hash_extender" %}

### Tool - HashPump

{% embed url="https://github.com/bwall/HashPump" %}
