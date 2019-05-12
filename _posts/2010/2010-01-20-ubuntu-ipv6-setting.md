---
title:  "ubuntu下ipv6设置"
date:   2010-01-20 11:19:10 +0800
---

ubuntu默认支持ipv6，只需配置地址即可。

vi /etc/network/interface
加入
iface lo inet6 loopback
iface eth0 inet6 static
        address 1:2:3::4
        netmask 64

重启
/etc/init.d/networking restart

测试
ping6 1:2:3::1

如果有跑apache，apache2指定了监听的ip要去修改，/etc/apache2/ports.conf  的Listen参数。
重启，必须
apache2 restart
而不是简单reload，否则监听端口无法生效

测试
netstat -tulpn | grep :80
必须有tcp6输出

telnet -6 1:2:3::4 80
如果无法访问，看看是否有ufw防火墙，有的话去修改使得ufw支持ipv6。
vi /etc/default/ufw enable
ipv6=yes

原先添加的规则只是针对ipv4，需要重新删除再添加一次，最后重启
/etc/init.d/ufw reload
使用telnet测试
