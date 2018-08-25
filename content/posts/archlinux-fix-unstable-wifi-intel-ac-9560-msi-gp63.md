+++
title = "Archlinux Fix Unstable Wifi Intel AC 9560 MSI GP63"
date = 2018-08-25T14:00:52Z
tags = []
categories = []
+++

### Wifi connection stopped after a while
After using the MSI GP63 a while, there was an issue came up with the wifi, the wifi would suddenly stop. The wifi connection would remain connected and there is no error can be found at anywhere. However, there is nothing is transmitting.

And it is very unfortunately that there is no similar report for this laptop and this wifi card (Intel AC 9560). But it is lucky that after some trial and error hacking with the options for iwlwifi, seems like I can keep it stable running (hibernate is also fine). I cannot tell whetherthe problem is related to the driver or firmware. But hopefully it will be fixed in the future.

Create the conf file for iwlwifi as below and add the "options iwlwifi swcrypto=1" into it.
```
sudo vim /etc/modprobe.d/iwlwifi.conf
options iwlwifi swcrypto=1
```
and reboot or reload iwlwifi module. (pick the way easy for you)

### Final
Hopefully this problem will be fixed in the near future.
