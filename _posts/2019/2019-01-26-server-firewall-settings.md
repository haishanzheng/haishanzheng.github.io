---
title:  "服务器防火墙设置一网打尽"
date:   2019-01-26 15:28:09 +0800
---

防火墙是服务器防护的第一道关卡，防火墙设置得好不会让你飞起来，但是至少不会让你跌的太惨，有太多的问题其实只要一个简单的防火墙即可规避。当然传统意义的防火墙只能控制端口的开启和关闭，端口开启以后上面服务的漏洞和更复杂的HTTP类似的更多的安全问题这些防火墙无法避免。

**是否配置防火墙是一个最基本的安全意识**，是是否有贯彻最小权限原则的体现。管理一台服务器，首先要看防火墙开放情况，有些购买的设备本身无法设置防火墙，就需要上一层交换机或者防火墙协助防护。防火墙设置是越靠近越好，规则越细越好，纵深防御路上数据中心和互联网边界上的较宽的防火墙规则也有存在的必要。



## 防火墙配置的多个途径

- GUI：通过GUI界面编辑防火墙，好处是简单，不需要懂太多的防火墙知识，有验证，效果跟visudo一样。缺点就是编辑速度慢，观察的时候不是太直观。
- CLI：设置速度快，有一定的验证，需要熟悉防火墙语法。
- 配置文件：直接修改配置文件是速度最快的方法，可以进入DevOps自动化部署流程，缺点是直接修改配置文件没有验证。

对于以上几种配置途径，各有优缺点，GUI降低了管理员的能力要求，但是也会带来一些麻烦，如果界面做得不好，对于切换规则和查错速度没有直接命令或者操作log来的快速。如果对防火墙规则不是太熟悉，GUI配置后生成的规则是学习最好的材料。如果可能，最后应当做成配置文件或者bat、sh等命令行集合，保证幂等，可重复运行，会大大降低配置和切换的工作量。

## 失效的防火墙

防火墙配置很简单，但是防火墙失效的原因也很多：

- 一条过宽的规则隐藏在几十条粒度不同的防火墙规则内
- 防火墙被乙方或者其他人为了调试等原因关闭而没有再开启
- 防火墙本身规则不了解，没有设置默认DROP或者忽略了IPv6协议
- 配置文件格式错误导致服务无法启动或者应用了旧的规则
- 应用了错误的情景模式，比如Windows规则配置的“公用网络”，但是某次启动后应用了“专用网络”。
- 多次根据不同重保规则切换防火墙导致规则错乱，遗漏或多余。
- 实时生效和持久性生效的区别，有些规则启动后就消失了。

所以验证防火墙是否正确不是去阅读规则，而是应当查看放行日志，采用nmap或者其他端口扫描工具定期扫描，如果写过程序就知道，这是类似TDD的做法，首先写一堆防火墙检测规则AssertTrue/AssertFalse，然后再配置防火墙，直到所有所有测试通过。

## 防火墙配置步骤

所以建议的设置防火墙步骤是

- 通读防火墙文档，熟悉所有设置
- reset防火墙配置，排除干扰
- 使用GUI或者CLI配置，并观察生成的配置文件，作为模板
- 精确识别服务器需要开放的端口和开放的范围
- 根据模板修改，合并到自动化部署流程内
- 定期使用nmap扫描确认
- 配合业务的查错

防火墙涉及到出和入，一般出我们不会限制，入也会涉及到状态，比如调整系统时间的NTPD，为了规避时间的跳跃修正同步系统时间，跟ntpdate不同的是，他需要双向开放防火墙。还有对于允许单播对多播或广播响应，一个通常的错误有时候在于应用（类似VMWare vSAN）选择了错误的多播地址而导致信息泄露。这里其实我也不是太明白，等明白了以后再说。

## 突然接手一台没有防火墙配置的服务器

如果你突然接手一台没有配置防火墙规则的服务器，比较暴力的做法是直接全部干掉，等业务运维人员折腾很久后跳起来来找你。或者考虑友好性，首先应当使用tcpdump收集几天的现网流量通讯情况，再配置一个较大的规则，保证业务的正常运行，再慢慢根据业务的运行情况和阻断日志收缩到最终的细的规则。

## 应用申明端口协议规则



一个有修养的应用，应当有对需要的端口和协议进行声明的文档，并且这些声明可以直接拷贝进入防火墙的配置内，UFW有applications.d，firewalld也有services，最不济也应当有死的说明文档。就类似Android开发程序的权限申明，或者SELinux，SELinux也是跟防火墙的命运一样，处于经常被乙方关闭的状态，实际上去匹配SELinux规则没有那么复杂，他跟防火墙一样，也有阻断Log，一个应用应当主动向SELinux申请最小需要的资源，包括端口开放，文件读写路径等，对其他人快速了解应用结构也是一个非常好的途径。**没有开启SELinux的原因就是懒，对安全没有敬畏之心**。

## 防火墙配置实例

Windows客户端的防火墙比较复杂，“允许应用通过 Windows Defender 防火墙进行通信”的条目过多导致审查工作量太大。如果一个程序需要对外通信，会弹出UAC类似的界面让我们选择是否允许，一般的选择都是允许，但是这个允许动作实际上开放范围比较大，比如我最近重装系统就发现，Windows Subsystem for Linux的SSHD、syncthing、Mouse without Borders等应用以应用程序而不是端口建立规则的权限开放过大，而且卸载一个应用后防火墙规则不一定会自动删除，这些需要手动调整。所以这里不谈客户端，只说服务器，包括Windows、Ubuntu ufw、CentOS6 iptables/ip6tables、CentOS7 firewalld等。

在下面的实例里，都假定管理员IP 192.168.1.100 需要访问服务器所有端口，某个其他服务器IP 192.168.1.200 要访问 1433/tcp 端口，服务器对外开放 80/443 端口。

## Windows 2k3 GUI

Windows 2k3早已经过了EOL，理论上不应当使用它，但是现实里还是有大量内网还在使用，Windows 2k3没有防火墙，只有控制面板里面带的“本地安全策略”的“IP安全策略”可以达到同样的效果。关于GUI如何配置这里不再细说，可自行Google。

## Windows 2k3 CLI

参考我2005年写过的，https://dog.xmu.edu.cn/2005/09/13/ip-ipsec.html，将以下命令存成.bat，重复运行。



    REM =================开始咯================
    REM =================幂等的关键================
    netsh ipsec static ^
    delete policy name=Haishion

    netsh ipsec static ^
    add policy name=Haishion

    REM 添加2个动作，禁止和允许
    netsh ipsec static ^
    add filteraction name=Perm action=permit
    netsh ipsec static ^
    add filteraction name=Block action=block

    REM 首先干掉所有访问
    netsh ipsec static ^
    add filterlist name=AllAccess
    netsh ipsec static ^
    add filter filterlist=AllAccess srcaddr=Me dstaddr=Any
    netsh ipsec static ^
    add rule name=BlockAllAccess policy=Haishion filterlist=AllAccess filteraction=Block

    REM 开放管理员192.168.1.100无限制访问
    netsh ipsec static ^
    add filterlist name=UnLimitedIP
    netsh ipsec static ^
    add filter filterlist=UnLimitedIP srcaddr=192.168.1.100 dstaddr=Me
    netsh ipsec static ^
    add rule name=AllowUnLimitedIP policy=Haishion filterlist=UnLimitedIP filteraction=Permit

    REM 开放192.168.1.200可以访问1433端口
    netsh ipsec static ^
    add filterlist name=SomeIPSomePort
    netsh ipsec static ^
    add filter filterlist=SomeIPSomePort srcaddr=192.168.1.200 dstaddr=Me dstport=1433 protocol=TCP
    netsh ipsec static ^
    add rule name=AllowSomeIPSomePort policy=Haishion filterlist=SomeIPSomePort filteraction=Permit

    REM 开放80,443端口
    netsh ipsec static ^
    add filterlist name=OpenSomePort
    netsh ipsec static ^
    add filter filterlist=OpenSomePort srcaddr=Any dstaddr=Me dstport=80 protocol=TCP
    netsh ipsec static ^
    add filter filterlist=OpenSomePort srcaddr=Any dstaddr=Me dstport=443 protocol=TCP
    netsh ipsec static ^
    add rule name=AllowOpenSomePort policy=Haishion filterlist=OpenSomePort filteraction=Permit

## Windows 2k8以上 GUI

- 首先建议“还原默认设置”
- 然后进入“允许程序或功能通过 Windows 防火墙”。对每个进行设置。
- 接着打开“高级设置”，点击“入站规则”，然后“按配置文件筛选”，“按状态筛选”，进行调整。

## Windows 2k8以上 CLI

    netsh advfirewall reset
    netsh advfirewall firewall delete rule name=all

    netsh advfirewall set allprofiles state on
    netsh advfirewall set allprofiles firewallpolicy blockinbound,allowoutbound

    netsh advfirewall firewall add rule name="Trust Admin" dir=in protocol=tcp remoteip=192.168.1.100 action=allow

    netsh advfirewall firewall add rule name="Trust web to 1433" dir=in protocol=tcp remoteip=192.168.1.200 localport=1433 action=allow

    netsh advfirewall firewall add rule name="Open Port" dir=in protocol=tcp localport=80,443 action=allow

## Ubuntu ufw CLI

    ufw reset
    ufw enable
    ufw default deny

    ufw allow from 192.168.1.100
    ufw allow proto tcp to any port 1433 from 192.168.1.200

    ufw allow 80/tcp
    ufw allow 443/tcp

## Ubuntu ufw 配置文件

    /etc/ufw/user.rules

    /etc/ufw/user6.rules

这里面的规则建议根据以上CLI生成后保存成模板进入自动化部署，由于内容较多，这里不再列出。

## CentOS6 iptables/ip6tables GUI

    system-config-firewall-tui

## CentOS6 iptables/ip6tables CLI

    iptables -F
    iptables -P INPUT DROP

具体规则可参考下面的配置文件，前面加上iptables即可

    iptables -L
    service iptables save

继续对ip6tables操作

## CentOS6 iptables/ip6tables 配置文件

    /etc/sysconfig/iptables
    /etc/sysconfig/ip6tables

    # Firewall configuration written by system-config-firewall
    # Manual customization of this file is not recommended.
    *filter
    :INPUT ACCEPT [0:0]
    :FORWARD ACCEPT [0:0]
    :OUTPUT ACCEPT [0:0]
    -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
    -A INPUT -p icmp -j ACCEPT
    -A INPUT -i lo -j ACCEPT
    # custom begin
    -A INPUT -m state --state NEW -m tcp -p tcp -s 192.168.1.100 -j ACCEPT
    -A INPUT -m state --state NEW -m tcp -p tcp -s 192.168.1.200 --dport 1433 -j ACCEPT
    -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
    -A INPUT -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT
    # custom end
    -A INPUT -j REJECT --reject-with icmp-host-prohibited
    -A FORWARD -j REJECT --reject-with icmp-host-prohibited
    COMMIT

## CentOS7 firewalld CLI

    firewall-cmd --state
    firewall-cmd --get-default-zone

    firewall-cmd --permanent --zone=public --add-rich-rule='rule family=ipv4 source address=192.168.1.100 accept'
    firewall-cmd --permanent --zone=public --add-rich-rule='rule family=ipv4 source address=192.168.1.200 port port=1433 protocol=tcp accept'
    firewall-cmd --enable service=http
    firewall-cmd --add-port=443/tcp

    firewall-cmd --reload

    firewall-cmd --list-all


## CentOS7 firewalld 配置文件

    /etc/firewalld/zones/public.xml

    <?xml version="1.0" encoding="utf-8"?>
    <zone>
        <short>Public</short>
        <description>For use in public areas. You do not trust the other computers on networks to not harm your computer. Only selected incoming connections are accepted.</description>
        <rule family="ipv4">
            <source address="192.168.1.100"/>
            <accept/>
        </rule>
        <rule family="ipv4">
            <source address="192.168.1.200"/>
            <port protocol="tcp" port="1433"/>
            <accept/>
        </rule>
        <port protocol="tcp" port="80"/>
        <port protocol="tcp" port="443"/>
    </zone>

## 最后

重启，确认服务正常，配置正常，nmap定期扫描。
