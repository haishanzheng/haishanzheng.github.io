---
title:  "高校邮件系统配置相关"
date:   2018-07-01 15:28:09 +0800
---

宋崟川、中科大张焕杰、华南理工黄建波、上交大章思宇、difan@google.com均对本文有贡献。

邮件有Web界面、POP3、SMTP、IMAP等多个服务，很多学校是部署在一个服务器上的，所以就会给人一个误解，一个Mail服务器就应当有这么多服务，其实这些服务都是可以分开部署的，而且分开部署后逻辑更清晰，比如Web流量就可以很简单去过反向代理，也可以根据访问量调整服务器数量。

就好像DNS，原先我们也是只有一台，上面做权威和缓存，后来出来了DNS反射攻击，于是剥离了权威和缓存，2台权威面对全世界

![](/images/2018/twoagainsttheworld.jpg)

，多台缓存只限制校内才能连接。有些高校更严格，缓存再分为缓存和递归，缓存只接收递归的结果，由递归服务器对外查询。确实我们学校以前也发生过一件事情，就是外网断了，缓存+递归服务器对外查询超时导致等待一千的连接限制用完，无法对校内进行解析，如果这时候再分缓存和递归再加张焕杰的校内根域名服务器镜像还是有可能让用户访问到变成局域网的校内的域名。

所以类比来说，HTTP、POP3、SMTP、IMAP是邮件服务器对服务器内拥有账户的用户提供的服务。具体来说，这些服务是面对校内老师学生的。还有内部的管理WebUI、API等如果有可能也监听不同的IP或端口。

而对互联网来说，发信和收信都是基于SMTP协议，他们只想知道2个事情：

- 域的MX记录是什么，这个决定了别人要投递给你是投递到哪个服务器IP。
- 你对外发送的IP是什么，这个决定了对方要接收你的邮件要来检查FCrDNS判断是否垃圾邮件。

所以以上几个都可以剥离出来不同的IP地址、域名、端口来监听。举个例子，基于域名：

- mail.xmu.edu.cn，对内服务，提供HTTP
- pop3,smtp,imap.xmu.edu.cn，对内服务，提供POP3、SMTP、IMAP。
- mx.xmu.edu.cn，对互联网服务，接收邮件，提供SMTP。
- 对外发送邮件IP，使用SMTP协议。对外发送还可以走relay服务器，买海外转发服务等。

## 根域的DNS配置

对于形如example@xmu.edu.cn的邮件地址来说，宣告接收邮件的服务器的IP地址设置在xmu.edu.cn的MX DNS RR上。

- xmu.edu.cn的A记录为了SEO，一般是建议跟www.xmu.edu.cn一致，并做301跳转。原先我们为了用户方便直接指向邮件的A记录，但是后来一般浏览器会自动前面给你加www，然而为了用户习惯一直不敢变更直到最近。
- SPF记录。SPF如果要写得完美一点，可以考虑下面场景：如果你老师和学生用不同的域，比如xmu.edu.cn和stu.xmu.edu.cn，则这2个域的SPF都include spf.xmu.edu.cn（类似引用一个变量），在spf.xmu.edu.cn里面才真正允许发信服务器的A/AAAA记录。所以bind9写法就是 

        @   TXT "v=spf1 include:spf.xmu.edu.cn -all"
        stu TXT "v=spf1 include:spf.xmu.edu.cn -all"
        spf TXT "v=spf1 ip4:1.2.3.4/32 ip6:2001:da8::1/128 -all"

写完可以在 https://www.kitterman.com/spf/validate.html 这里检测下。

## PTR的DNS配置

SPF是为了解决SMTP发信人伪造问题，而FCrDNS是为了解决你确实对这个IP和他宣称的域名对应关系有控制权。

https://en.wikipedia.org/wiki/Forward-confirmed_reverse_DNS

所以发信服务器的IP地址一定必须设置反向解析。PTR指向什么域名无所谓，配置类似：

    4 PTR mta.xmu.edu.cn.

IPv6的RBL（Real-time Blackhole List）列表是否已经建立完善了不确定。邮件加IPv6支持是否有其他副作用不确定。

## 高校如何申请IPv6的PTR解析

PTR一般只用在邮件的反向查询和traceroute里。为了让邮件系统完全支持IPv6，你需要申请到PTR解析的权限，准备2台有IPv6的权威DNS服务器，在

“http://www.nic.edu.cn/RS/templates/cindex.html”  “IP6.ARPA 注 册 表 格” 里申请，里面的netname可以在站内的whois查询到。

## SSL

相对于HTTP 80端口301到HTTPS 443端口的协议升级，SMTP等没有这样子的机制，你在给邮件启用TLS后可以：

- 广而告之，建议大家使用TLS加密，最安全。定期根据25端口访问日志邮件推送对方建议使用TLS。
- 自动配置，新配置用户默认起SSL。不是所有客户端MUA都支持。
- 机会性加密，在SMTP 25端口直接协议升级到TLS，https://starttls-everywhere.org/。机会性加密可以预防被动监听，无法预防MITM。

## 端到端加密

使用TLS解决不了端到端加密问题，邮件PGP一直没有流行起来，如果类似iMessage那样子端到端加密，你就丧失了服务端给你提供过滤垃圾邮件的功能。当然，任何不开源的端到端加密方案讨论都是没有意义的，最近很火的初始化打开VIVO NEX摄像头的Tg就是开源的端到端加密IM。

## 自动配置

邮件客户端在用户配置新账户时有自动发现功能，比如 https://web.archive.org/web/20150817115525/http://moens.ch:80/2012/05/31/providing-email-client-autoconfiguration-information/ 这篇文章提到的Thunderbird、Outlook、Apple Mail等的自动配置机制。Outlook更详细的发现流程如下 https://support.microsoft.com/en-us/help/3211279/outlook-2016-implementation-of-autodiscover 。可以通过配置autodiscover自动宣告SMTP和POP3域名，还可以自动配置SSL，当然如果没有自动发现，你的域名是标准的pop3,smtp,imap一般客户端也都可以正确猜出来。
