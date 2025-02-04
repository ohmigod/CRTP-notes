---
description: Windows and Linux Port Forwarding
---

# Port Forwarding

### SSH Port Forwarding

Imagine we discovered an intranet network on a compromised machine. We can use SSH tunneling to establish connection from our Kali to this machine (using the compromised machine as a tunnel):

```bash
ssh -N -L 0.0.0.0:$PORT:$IPADDRESS:$PORT $USER@$IPADDRESS2
#-N: No command prompt
#-L: Local port forward
#$PORT: Port to forward
#$IPADDRESS: IP address of the internal network machine
#$USER: User on the compromised machine to log in as SSH
#$IPADDRESS2: IP address of the compromised machine
```

### SSH Remote Port Forwarding

Same as SSH Port forwarding but in this case executed inside the compromised machine, to bind a port from this machine to our Kali machine:

```bash
ssh -N -R $IPKALI:$PORT:127.O.O.1:$PORT kali@$IPKALI
```

Then we can interact with the remote port from our attacker machine:

```bash
nmap -sS -p$PORT 127.0.0.1
```

### SSH Dynamic port Forwarding

The following method will be used to get connectivity to all the ports from our Kali machine to the Internal machine, using the compromised linux client as intermediary.

First of all we need to configure Proxychains:

```bash
nano /etc/proxychains.conf
#add the following line imder [ProxyList]
socks4 127.0.0.1 8080
```

Now create the Dynamic port forwarding:

```bash
ssh -N -D 127.0.0.1:8080 $COMPROMISEDUSER@$COMPROMISEDIPADDRESS
```

**Note:** In order to effectively use this method we need to start the applications through proxychains. For example:

```bash
proxychains nmap -sS -p- $INTERNALNETWORKIP
```

### SSH Dynamic for Windows - PLINK

We can use the above techniques in Windows thanks to the PLINK.exe tool:

{% code overflow="wrap" %}
```bash
#-R: Reverse, but we can use -L for local and -D for dynamic:
cmd.exe /c echo y | plink.exe -ssh -l $USERNAME -pw $PASSWORD -R $KALIIP:$PORT:127.0.0.1:$PORT $KALIIP
```
{% endcode %}

### Windows port Forwarding - NETSH

Imagine we have compromised a Windows client and it's connected to an internal network which we do not have access from our Kali machine. We can pivot to any internal machine thanks to netsh:

**Note:** Execute all the commands on the compromised machine.

First of all we need to add the following firewall rule to enable inbound traffic from the desired port:

{% code overflow="wrap" %}
```bash
netsh advfirewall firewall add rule name="forward_port_rule" protocol=TCP dir=in localip=$COMPROMISEDIPADDRESS localport=$LOCALPORTTOUSE action=allow
#LOCALPORTTOUSE: If we want 445, use 4455. If we want 3306, use 33306...
```
{% endcode %}

Now execute the following netsh command:

{% code overflow="wrap" %}
```bash
netsh interface portproxy add v4tov4 listenport=$LOCALPORTTOUSE listenaddress=$COMPROMISEDIPADDRESS connectport=$INTERNALPORT connectaddress=$INTERNALIPADDRESS
```
{% endcode %}

Now we can interact with the internal machine port using the IP address and port of the compromised machine.

### Tunneling using HTTP protocol - HTTPTunnel

We will rely on HTTPTunnel to encapsulate our traffic within HTTP requests, creating an “HTTP tunnel”.

**Scenario:** Compromised Linux machine that have access to an internal network which is not accessible from our Kali machine. Also, only http traffic is allowed.

```bash
#Local port forward to the internal network:
ssh -L 0.0.0.0:8888:$IPINTERNALNETWORK:$PORTINTERNALNETWORK $USER@127.0.0.1

#Use HTTPServer to redirect incoming traffic from 1234 to 8888 port:
hts --forward-port localhost:8888 1234
```

In our Kali machine:

```bash
htc --forward-port 8080 $COMPROMISEDIPADDRESS:$PORT
```

Now everything will be encapsulated as HTTP protocol to evade Deep Packet Inspection.

### Port Forwarding using RINETD

**RINETD** is a tool that helps us to achieve port forwarding. Once this tool is installed we need to add the forwarding rules on the next file:

```bash
#file: /etc/rinetd.conf
#Add the following line under forwarding rules:
0.0.0.0 $PORT $IPADDRESS $PORT

#For example, forward any incoming web access to the 1.2.3.4 server:
0.0.0.0 80 1.2.3.4 80

#Once done, restart the service and watch if it's active:
sudo service rinetd restart
ss -antp | grep "$PORT"
```
