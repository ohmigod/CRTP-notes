---
description: Origin IP discovery methods
---

# Origin IP Discovery

Modern website architecture frequently involves multiple systems between the attacker and the real web server IP address. More often than not, the IP address of a domain name points to a CDN/WAF IP (Cloudflare, Cloudfront, Akamai...), a load balancer IP, or any other system IP.

Most of the time, the origin server relies completely on that intermediate systems, forgetting that a potential attacker can **obtain the origin IP address** using various sources/techniques. This is every interesting from an attacker perspective since there are big chances that the origin IP doesn't implement any security mechanism (like a WAF). This **will lead to a** **WAF bypass**.&#x20;

Here are some of the methods to discover the origin IP.

### Historial DNS

Some websites record historical changes to DNS records (A, AAAA, MX...) that can reveal the origin IP address. Some of this websites are:

* [SecurityTrails](https://securitytrails.com/dns-trails)
* [DNS History](https://dnshistory.org/)
* [IP History](https://viewdns.info/iphistory/)

### Search engines

Search engines can also reveal the origin IP by searching for the domain name itself or any other related dork. Some search engines are:

* [Shodan](https://www.shodan.io/)
* [Censys Search](https://search.censys.io/)
