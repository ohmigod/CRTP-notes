---
description: Padding Oracle Attack
---

# Padding Oracle

> Padding Oracle Attack target CBC-mode decryption functions operating with PKCS7-mode padding. A Padding Oracle can reveal if the padding is correct for a given ciphertext.

### Detection

* **Cookies:** If the cookie value is not changing after logging out or re-login.&#x20;
* **Request parameters:** If the request parameter looks like random chars like a ciphertext, try to manipulating the length by removing the one or more characters and analyze the response.&#x20;
* **Hidden field values.**&#x20;
* **Verbose errors** (like "invalid padding" or similar).&#x20;

### Tool - padbuster

PadBuster is a Perl script for automating Padding Oracle Attacks. PadBuster provides the capability to decrypt arbitrary ciphertext, encrypt arbitrary plaintext, and perform automated response analysis to determine whether a request is vulnerable to padding oracle attacks.

{% embed url="https://github.com/AonCyberLabs/PadBuster" %}
