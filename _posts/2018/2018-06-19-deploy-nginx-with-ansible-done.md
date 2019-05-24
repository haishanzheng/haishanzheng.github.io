---
title:  "继续说IPv6、HTTPS、HTTP/2，系列完结吧"
date:   2018-06-19 15:28:09 +0800
---

放假前介绍了《5分钟让你的老旧网站支持IPv6、HTTPS、HTTP/2，不能再多了》，如果你跃跃欲试，那我要告诉你，**对不起，我欺骗了你纯洁的心**。

如果你问我反代要怎么做，比较政治正确的说法我会告诉你去买大厂的应用交付设备，启用一键断网，并购买完善的维保服务，接收厂商定期的更新和威胁情报，并最终在出安全问题后甩锅给设备和厂商，以避免你在余生中为自己的抠门和不专业而后悔。

为什么？以我的repo为例，如果你真的反代了你全校的网站，那这个服务器一定要三级等保合规，你需要在上面安装防病毒、恶意代码检测、完整性检查、监控、资源限制、审计，你的日志需要到远程保留3个月，你必须时时更新安全补丁防止CVE漏洞，在Nginx新版发布后必须检查changelog并关闭不必要的功能。为了防止单点故障，你应该需要2台反代，上面安装Keepalived做高可用。为了方便分析，你还需要分析日志。

这些我改改代码都可以在10分钟内让你完成，但是我的代码你自己审计了么？我是否了解了所有的坑？我是否会偷偷插入一些控制你全部站点的代码？就比如更新免费的HTTPS证书，你在反代上去下载一个.sh文件就跑，安全么？如果为了隔离这个风险，我会使用一台不可变服务器，3个月定期部署他并让他跑起来，使用DNS challenge，在最后得到证书后scp到反代上并且销毁那个服务器。这么折腾为什么不直接花几千元买个一年有效期更高等级的EV证书？

就算你信任我，但是我也不是铁板一块，如果我被黑了呢？

所以除非你有足够的自信，否则不推荐，当然张焕杰和我的文档、repo也不是毫无用处，至少你可以借此揭开反代后面神秘的面纱。

泼了冷水，让我们继续来学习一下其他坑吧。写的比较散，想到哪写到哪，争取这个系列就完结了。

## 部署反代不是无创的

### IP地址传递

部署反代是一定需要的，反代的工作原理导致了你的应用无法得到真实客户端IP地址，所以反代一般会把X-Forwarded-For、X-Real-IP等参数往应用后面传，又由于这个参数是放在Header里的，所以必须应对伪造问题。

而且反代有时候是多级的，所以反代一定要找到自己的定位，在何时重置X-Forwarded-For、X-Real-IP字段，何时接受信任上级的字段。而应用必须更改自己实现获取客户端IP地址的机制，只接收信任反代发送的Header。这个去年底我写了个案例，就是根据伪造X-Forwarded-For字段来欺骗应用突破IP地址授权验证的。

应用的Web服务器日志配置也必须修改，否则只能得到反代的地址。有些关于X-Forwarded-For 的网上配置是错误的。有些是简单粗暴用X-Forwarded-For替换掉%h，比如

    LogFormat "%h %l %u %t \"%r\" %>s %b" common
    LogFormat "%{X-Forwarded-For}i %l %u %t \"%r\" %>s %b" common

但是这样子也不能解决伪造X-Forwarded-For的问题，所以为了统计分析方便，不建议你直接去更改Log格式，建议你应当生成2个日志，一个是%h的，一个是%{X-Forwarded-For}i。各自分析。

而如果你在系统里面配置了诸如fail2ban、安全软件等根据IP来封禁的全部必须做相应调整。

### HTTP/2

以Nginx来做反代，Nginx对后端backend HTTP/2协议是无视的，这是由于HTTP/2协议本身特性造成的。所以你的反代必须清理所有backend传过来跟连接相关的Header。举个例子：

假设你用Chrome使用HTTP/2协议去反代取某个backend的网页，然后反代以HTTP/1.1去backend取网页，这时候支持HTTP/2的backend发现反代的协议好老，会给反代发送“upgrade: h2,h2c”头部，就是告诉反代，你落伍了，该升级了，如果你的反代不清理这个头部proxy_hide_header，那你的Chrome会拿到这个字段，他会觉得很莫名其妙。

## IPv6

其实我觉得部署反代来解决IPv6问题是不是一个“上有政策下有对策”的做法，长期倒是会损害IPv6的发展，何时数据中心敢上纯IPv6环境呢？

或者IPv6要真正推广需要有应用厂商帮忙来推，比如某个系统很早前就号称支持IPv6，在很多学校部署得也很多，但是基本上都不用IPv6，不知道是什么问题。确实，多一条路就多一个风险点，而上了IPv6又不会让你飞起来，大家就没有动力来推动这个事情。

说到这个系统，那继续说下这个系统的安全问题吧，有些是部署的问题，有些是实现的问题。我不是具体负责这个系统的，我也只是零星使用时发现的。**所以你不要让我来看或者使用你的系统，我会发现一些乱78糟的问题让你我都很尴尬**。

- 密码防止暴力破解设计很奇特。如果你对某个账户破解密码超过一定阈值后不是告诉你被锁定而是不管你输入正确还是错误的密码都告诉你密码错误，对用户很困扰，对攻击者也不友好。我曾经写信去咨询希望能改变方式或者提供开关让我们自己选择，但是没有下文。
- 直接把认证信息加在URL里，类似 http://dog.xmu.edu.cn/index.php?login-user-hash-is=GUID ，这样子也是非常不安全的，虽然这样子是对cookiless友好的，但是如果没有HTTPS，地址很容易被嗅探，并且地址也会加入Web服务器的Log内，甚至都无法预防有人站在你身后拍照你的屏幕传给别人。后来指出后，加了个同来源IP地址的验证，也就是即使你拿到地址后，必须在同一个IP才能访问。但是这个也不靠谱，在同一个物理NAT环境或者拨同一个VPN还是有可能被攻击。而且在部署或者代码里没有对Referrer Policy进行指定，浏览器的默认Referrer Policy是no-referrer-when-downgrade，也就是，你在系统内点击链接后会把GUID传给黑客的Web服务器。再后来是加了Cookie，安全了。其实我现在也不是太明白，有了Cookie后为什么还要有那个login-user-hash-is，可能是为了兼容旧应用的API调用吧。
- 管理员界面和API调用绑定在普通用户同一个Web服务器端口内，如果你的超级用户密码强壮，系统本身没有问题这个是问题不大的，我们为了保险还是加了一层管理员IP限制，很多学校是没有的。而且API认证模式是使用IP地址认证。

希望今后这个系统能真正对部署的安全梳理一层，提高各个学校的安全性吧。

关于IPv6地址规划，我不是搞网络的，我是搞应用的，所以我不是太懂，宋崟川写了个 https://www.ipv6-cn.com/tutorials/ipv6-address-allocation-n-assignment.html 可以学习，我是看不懂的，但是如果你是使用IPv4来映射出IPv6地址的，虽然简单但是感觉可以变了。

IPv6巨大的地址池给我造成的问题就是，原先使用IP地址来做诸如投票IP地址唯一性判断方法有点失效了。攻击来源IP非暴力破解的判断策略也需要更改了。

### HTTPS

HTTPS可以提供保密性、完整性，简单给Web网站加个HTTPS可以解决大部分问题，如果你想再上几层，还有其他可以改进：

- DNS权威服务器要上DNSSEC，免得DNS就被人篡改了。
- 为了防止诸如以前的某些厂商等让用户安装他们的根证书，然后被黑导致MTTM攻击，可以在DNS上加CAA，只允许某些CA给你某个域名签名。
- 目前浏览器默认是访问80端口的，未来可能会改变，所以一般网站的做法都是先访问HTTP然后301到HTTPS，这中间是可能被攻击的。所以你应当加HSTS、hsts preload list、STS-in-DNS等改变这个流程。
- 网页如果加了HSTS一年，但是黑客可以利用NTP攻击让客户端电脑时间推后一年导致HSTS无效。有人这么无聊么？反正都考虑到吧。
- HTTP服务器配置要防止SSL协议和加密方法降级攻击。

### HTTP/2

关于HTTP/2的介绍网络上很多，我就不搬运了，改变很大，基本上目前HTTP/2都必须前置有HTTPS，所以有些调试技巧失效了，而且你以前掌握的一些HTTP/1.1的优化奇技淫巧也都没用了，重新学习吧。