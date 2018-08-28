+++
title = "Arch Linux Disable Ipv6"
date = 2018-08-28T08:40:28Z
tags = ["linux", "wifi", "ipv6"]
categories = []
+++

### Diable Ipv6
There are few things I usually do when installing linux. One of them is disabling ipv6. If you also feel like to disable but not sure how, here is what I did.

First, add below to grub for starting kernel, and remember to run grub-mkconfig.
```
ipv6.disable=1
```

Second, update the dhcp config file (/etc/dhcpcd.conf) with below.
```
ipv4only
```

Reboot and try.

### Final
without updating dhcpcd config file would sometimes work, depends on the server that you connect to. I sometimes run into the problem that the dhcp server sending ipv6 info to me when initializing thewifi connection.
