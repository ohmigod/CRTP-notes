---
description: Command Injection
---

# Command Injection

> Command injection is an attack in which the goal is execution of arbitrary commands on the host operating system via a vulnerable application.

### Ways to concatenate commands

#### Windows & \*nix:

```bash
&
&&
|
||
```

#### \*nix-based:

{% code overflow="wrap" %}
```bash
;
Newline (0x0a or \n)

#You can also use backticks or the dollar character to perform inline execution of an injected command within the original command:
`command`
$(command)
```
{% endcode %}

***
