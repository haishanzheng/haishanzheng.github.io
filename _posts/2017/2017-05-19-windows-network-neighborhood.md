---
title:  "后WannaCrypt蠕虫勒索病毒445端口被封的网上邻居共享方法"
date:   2017-05-19 13:32:09 +0800
---


关于WannaCrypt蠕虫病毒，我就不科普了，大家被狠狠教育了一吨。很多人周一早上上班不敢打开电脑，有敬畏之心是对的。当然，我这里不说这个，我要说的是，网络管理员把445端口封禁了，我怎么办？？？

正如CERNET [关于教育网“大面积感染勒索病毒”不实报道的严正声明](https://media.weibo.cn/article?id=2309404107737301653165&from=timeline&jumpfrom=weibocom) 所说的

> 按照国际互联网技术规范，互联网TCP端口应为众多互联网应用提供开放的接口能力，除非紧急情况需要，互联网运营商一般不应该、也不会随意封堵主干网TCP端口。即便封堵，一般运营商也不会在主干网，而是由用户（如校园网或企业网）根据自身需要在接入主干网的边缘采取访问控制措施（例如封堵某些TCP端口）。

然而为了安全，相信很多校园网内网络管理员暂时都把不同网段的445端口封禁了。我严重依赖网上邻居，不管是在线看片娱乐还是工作，我需要打开很多个网上邻居窗口。因为我还是习惯在Windows下使用文本编辑器打开Linux服务器下的文件，或者Windows服务器本身根本没有安装好用的文本编辑器（由于最小化安装原则，不能安装），所以我需要顺手的文本编辑器打开远程的程序文件。

当然，使用网上邻居是否是一种不好的安全习惯有待探讨，运维区直接进入核心服务器网上邻居完全不经过堡垒机应该是不规范的。我也在考虑一些更为安全方便的文件共享和在线编辑功能，如果你知道，请告诉我。

Windows客户端有个奇怪的设定就是，基本上所有协议的端口都是可以修改的，然而只有445端口无法修改，如果你是使用Linux客户端，那445封禁对你来说根本不是问题，换个端口就好了。

废话了这么多，先说结论：通过Windows自带的Portproxy是可以突破网上邻居只能使用445端口的限制的，当然如果你的网络管理员从协议层而不是端口封的话，那算了。下面说具体方法。

__如果要开放网上邻居端口请注意加好防火墙。使用下面方法导致包括但不限于服务器被黑，文件被锁，内分泌失调等问题本人概不负责。__

![](/images/2017/portproxy/the-shawshank-redemption.jpg)


## Port proxy
[Netsh commands for Interface Portproxy](https://technet.microsoft.com/en-us/library/cc731068(v=ws.10).aspx)

> The Netsh Interface Portproxy commands provide a command-line tool for use in administering servers that act as proxies between IPv4 and IPv6 networks and applications. You can use these commands to establish proxy service in the following ways:

由于Windows客户端在运行里面输入\\\\a.b.c.d，后面不能加端口，（如果加了端口就是WebDAV了），所以首先需要突破这个，基本思路是，在本机再起个其他IP地址，在这个IP地址监听445端口，访问远程的网上邻居改为访问本机端口，通过本机Portproxy到远程4455端口，如果是Linux就OK了，Windows还需要再本机Portproxy到445端口。一图胜千言。

![](/images/2017/portproxy/portproxy-flow.jpg)

1，2，3就是Windows客户端访问Windows服务端网上邻居的网络走向数据流，中间只需要开放4455自定义端口。

## 开始设置
思路有了，那开始设置吧。Portproxy从Windows 2008就已经有了，所以Windows 8.1和Windows 10也是自带了。当然，也会有其他方法，包括直接VPN拨到服务器网段，通过其他代理转发等等这里不说明。使用本方法也会有副作用，包括命名管道不能再使用，某些应用（比如Acronis True Image）还是无法访问网上邻居等等。

## Windows服务端设置
添加一条Portproxy规则即可

	netsh interface portproxy add v4tov4 listenaddress=a.b.c.d listenport=4455 connectaddress=a.b.c.d connectport=445

### 排错
同客户端。

## Linux服务端设置

### samba改端口
	vi /etc/samba/smb.conf

在[global]内加入一行

	smb ports = 4455

重启smbd服务。

### 排错
查看samba服务是否正常。

	systemctl status smbd
	tail /var/log/samba/log.smbd

查看samba是否服务起在4455端口。

	lsof -i:4455

#### 防火墙和SELinux
注意打开4455端口的防火墙。如果smbd无法启动，提示监听时没有权限，可以确认下SELinux的问题。

如果SELinux为开启（推荐），查看smbd可以打开的端口

	semanage port -l|grep smb

增加打开4450端口：

	semanage port -a -t smbd_port_t -p tcp 4450

### 联调
使用tcpdump查看是否客户端有流量过来。

	tcpdump 'dst host x.y.z.z and ! dst port SSH_PORT'

## Windows客户端设置

### 干掉445

停止server服务。

	net stop server

干掉Server服务会导致一系列问题，[关于勒索病毒 Ransom:Win32.WannaCrypt 解决方案的总结说明](https://github.com/HiwebFrank/Blogs/blob/master/NetworkSecurityaboutWannaCry.md?from=timeline&isappinstalled=0)里面提到：

> 除非你知道会发生什么，否则不要随意关闭端口。 以下内容节选自 比我上面自吹的资历还深的朋友 MVP 胡浩 的文章《抵抗勒索病毒的正确姿势——不要上来就封端口！》 中的重点内容：

Server服务的描述：

> 支持此计算机通过网络的文件、打印、和命名管道共享。如果服务停止，这些功能不可用。如果服务被禁用，任何直接依赖于此服务的服务将无法启动。

因为Server服务bind的地址一定是0.0.0.0，所以只能是整个干掉，会导致你无法共享文件给别人，命名管道也不能用，比如SUBST虚拟磁盘无法使用。这个在我们管理远程3389时需要服务器客户端之间对拷文件时我一般会SUBST个虚拟盘，再挂接到服务端，比直接C、D盘整个挂接过去会更安全一点。

### 禁用Server服务
	sc config lanmanserver start=disabled

### 开启Portproxy

- 添加一个独立的IP地址，比如d.o.g.g
- 添加Portproxy，Portproxy在重启后依然会生效。

命令：

	netsh interface portproxy add v4tov4 listenaddress=d.o.g.g listenport=445 connectaddress=a.b.c.d connectport=4455

如果你网上邻居比较多，建议做个规划。我写了个脚本，放在最下面，参考。每次我只要添加一行，运行bat，自动执行以上2个步骤并且清理已经不再使用的IP和映射。

### 排错
查看Portproxy设置

	netsh interface portproxy show all

查看端口是否正确起来

	netstat -ano | findstr :4455

抓包 Wireshark

如果中间有什么异常，启用重启排障神器吧。


### 自动脚本
请确认你看得懂下面脚本再自动化操作。否则请手工操作。

{% highlight powershell linenos %}
@echo off

REM 为了防止UTF8问题，最好把网卡连接改为全英文，也就是“本地连接”、“以太网”改为全英文。
set INTERFACE_NAME=LocalNetCard
REM 默认IP不删除
set IPADDRESS_DEFAULT=x.y.z.z

setlocal enabledelayedexpansion

for /f "tokens=4 delims= " %%a  in ('netsh interface ipv4 dump^|find "%INTERFACE_NAME%"^|find "address"') do (
	if "%%a" equ "address=%IPADDRESS_DEFAULT%" (
		echo %%a is default ipaddress. Skiping delete.
	) else (
		echo delete ip%%a
		netsh interface ipv4 delete address name="%INTERFACE_NAME%" %%a
	)
)

netsh interface ipv4 add address name="%INTERFACE_NAME%" address=d.o.g.1 mask=255.255.255.0
netsh interface ipv4 add address name="%INTERFACE_NAME%" address=d.o.g.2 mask=255.255.255.0
netsh interface ipv4 add address name="%INTERFACE_NAME%" address=d.o.g.3 mask=255.255.255.0
netsh interface ipv4 add address name="%INTERFACE_NAME%" address=d.o.g.4 mask=255.255.255.0
netsh interface ipv4 add address name="%INTERFACE_NAME%" address=d.o.g.5 mask=255.255.255.0

echo ===========Now ip list:
netsh interface ipv4 dump|find "%INTERFACE_NAME%"|find "address="

netsh interface portproxy reset
netsh interface portproxy add v4tov4 listenaddress=d.o.g.1 listenport=445 connectaddress=a.b.c.1 connectport=4455
netsh interface portproxy add v4tov4 listenaddress=d.o.g.2 listenport=445 connectaddress=a.b.c.2 connectport=4455
netsh interface portproxy add v4tov4 listenaddress=d.o.g.3 listenport=445 connectaddress=a.b.c.3 connectport=4455
netsh interface portproxy add v4tov4 listenaddress=d.o.g.4 listenport=445 connectaddress=a.b.c.4 connectport=4455
netsh interface portproxy add v4tov4 listenaddress=d.o.g.5 listenport=445 connectaddress=a.b.c.5 connectport=4455
netsh interface portproxy show all

@echo on
{% endhighlight %}


## Linux客户端设置

smbclient -p 4455 //a.b.c.d/home -U DOG

mount -t cifs //a.b.c.d/home MOUNT_POINT -o username=DOG,port=4455,password=SOME_PASSWORD

## 好了
有什么问题告诉我，有什么不懂不要问我。

![](/images/2017/portproxy/brave-heart-freedom.jpg)

