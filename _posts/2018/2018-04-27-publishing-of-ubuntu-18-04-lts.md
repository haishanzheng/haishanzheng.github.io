---
title:  "Ubuntu 18.04 LTS发布了"
date:   2018-04-27 15:28:09 +0800
---

等了好久，Ubuntu 18.04 LTS终于发布了，代号Bionic Beaver（仿生海狸），接下来一大堆服务器要重新部署了。我一般不喜欢直接升级。。。

Ubuntu每年都会有个发布，4月份发布一个LTS版，10月份发布一个版。虽然是叫LTS版，支持5年更新，但是如果有新版本发布在登录后会给你提示可以更新到新版，对于我这个编译要打开-Wall(Warning all)甚至-Werror（all warnings being treated as errors）、写Python要打开PEP8严格的代码格式的人来说，是非常得难受，一定要升级到最新版。

由于还没安装，大概看了下，最新的Ubuntu把Python2拿掉了，默认也不安装Python。Server的安装过程也采用桌面端的Subiquity，如果要使用LVM还需要下载特殊的ISO。我只用Server不用桌面版。OpenJDK默认是10，9月份还要升级到11，PHP是7。swap不需要分区而是使用文件。

## CentOS比较

而CentOS7很克制，从2014年7月份发布到现在，离7年的生命周期还长着。CentOS一般发布后软件的大版本就不再更新了，只会backport一些小的新特性和安全补丁。CentOS会更稳定，兼容性高，Ubuntu软件会更新一些。**我还是更偏向Ubuntu一些**，自己用的会用Ubuntu，厂商部署的有些就用CentOS。

然而有个不好的是Ubuntu经常会更新内核安全补丁，比如Ubuntu内核已经追到4.15了，而CentOS7还是3.10。然后Ubuntu更新下来的旧内核为了保险不删除，导致如果你不经常apt autoremove，/boot空间会被占满，所以我现在一般/boot都要给1G。而且更新完内核还会要求你重启服务器。。。

Ubuntu的防火墙ufw很简单清晰，CentOS7引入的firewalld过于复杂，加入了区域的情景模式，一般服务器其实不会用到情景模式。

自动安全更新，Ubuntu在安装时可选，而CentOS7需要安装yum-core并且配置。

CentOS7默认建议使用xfs，Ubuntu还是ext4。

## 然而

我用过最奔放的开源软件是Gitlab，每年发布200多个版本，每次下载要300MB，使用Chef做自动化部署，每个工作日你都可以更新一次，更新一次下载一个小时。具体我不知道为什么打包了这么大，但是看起来**现在越来越推荐整个打包一起部署的方式了**，包括Ubuntu的Snaps，所有依赖包全部给你扔一起，安装完整个目录空间占用很大，有点绿色软件的感觉。还有docker，基本上就是打包整个操作系统整个运行环境了。
