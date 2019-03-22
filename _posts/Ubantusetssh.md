---
title: Ubantu  set ssh
date: 2018-10-06 16:07:40
categories:
- Linux学习
tags:
- Linux
---

1.You should download shadowsocks
```
apt install shadowsocks 
```

2.Write a congfig json
```
sudo vim /etc/ss-conf.json
```

3.The content of config
```
{
“server”:”0.0.0.0”,
“server_port”:8838,
“local_address”:”127.0.0.1”,
“local_port”:1080,
“password”:”123456”,
“timeout”:600,
“method”:”aes-256-cfb”
}
```
<!-- more -->
4.Start the service
```
sudo ssserver -c /etc/ss-conf.json -d start
```

5.Set the server started with the starting of ubantu

```
vi /etc/rc.local
```
add this before exit()
```
ssserver -c /etc/ss-conf.json start
```

