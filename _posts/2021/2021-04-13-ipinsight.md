---
title:  "网络空间测绘：IP洞察"
date:   2021-04-13 11:02:01 +0800
---

## 引子

当时钟刚刚转过0点，数据中心的空调压缩机在变频器的调节下保持长时间恒定速率运转，光纤的激光束频率变化不再频繁，Grafana的CPU idle曲线开始慢慢爬升，IPInsight的Jobs开始忙碌起来了。

beian从远处一个链接抓到了一堆数据，他小心地切掉未备案和没有生效的域名，再吃力得将数据倒进了Open Distro for Elasticsearch。

phpipam麻利地转进MySQL里，把IP分配记录捆绑成结实的body。“看，这个是IPv6”，果然，这个比其他个头都大，我拿了起来，因为没有NAT，通体显得更为透明，扒开表层的Packet，我果然看到了很多“:”，“以前这个品种还很少见，现在国家鼓励规模部署，多了”。我一不留神，IPv6从我手里滑了出去，“哈哈哈”，phpipam大笑起来，“因为有Privacy Extensions的存在，IPv6很滑，不好抓吧。”。

看着他们完成了工作，vmware卷起袖子，递归进每个vim.Folder，抓住每个vim.VirtualMachine，仔细清扫，把每个vm都打上vmwareToolsStatus标签，再统一送到ES去。这件事，他已经干了很久了，他自信地认为他甚至可以闭着眼睛完成。

高端的指纹识别往往只需要采用最朴素的扫描方式。nmap倚在线程池旁，离他放了30个并发请求去扫描服务器网段还不到几秒，看着subprocess capture_output到的数据在线程池里上下翻转，他按捺不住丰收的喜悦。很快，nmap用regex挑选出ipaddress，用强力气吹吹了吹，轻轻地放在盘子里，这些被精心挑选出来的IP地址，在静静等待更深入的转化。

tracepath对这些流程再熟悉不过了，从形态到内容，不管你是DNS，不管你是IPv4还是IP威武（v5），归根结底就是IP地址而已，只要是地址，他一定可以追踪到最源头。他一股脑儿把其他Jobs识别到的ipaddress拿来，细心记录下每一hop的网关地址和TTL。

whatweb和puppeteer没有等他们，在开始的第一秒他们就默默启动了，因为他们的活最苦最累，很多时候，他们发出的请求，只能默默等来30秒的timeout。

时间到了8点，上班的人们陆陆续续来了，郑工在态势感知平台里看到了一个IP，皱了皱眉头，在Controls里输入，Kibana赶紧把准备的数据关联在一起，五颜六色的Bar、Area、Line、DataTable、Metric、Pie在Dashboard跳出来。一切开始热闹起来了。

## Visibility

最近看了《数说安全》 [谭校长专访盛邦安全权小文：网络空间地图是网络安全乃至整个信息化建设的底图](https://mp.weixin.qq.com/s/PS8QaEvvRLqGIdZiFCSvow) 和 [网络空间地图技术与市场分析——探索网络世界的“藏宝图”](https://mp.weixin.qq.com/s/5f5FURD91sYcR_opgIS1pg)，和一些指纹识别的文章 [简单聊聊网络空间测绘纵横之道](https://mp.weixin.qq.com/s/aBvptjz9gzxG_lPBY8ECVA)、[网络空间测绘的生与死（五）：死亡篇](https://mp.weixin.qq.com/s/zAeys4pnTf9rC4KCzUU4iA)。我，以甲方局内人的角色也整了点东西。

在网络安全里，**看见**，是最重要的能力，你看到的越多，获取的信息越多，你越能掌握主动。

## 我的IP Insight是什么

在[高校安全设备之买买买攻略](https://dog.xmu.edu.cn/2019/06/30/strategy-of-buying-security-system.html)里，我说过，“否则安全设备买得越多，各个不同设备之间形成壁垒，成为信息化治理类似的烟囱架构。”，虽然我没有很多安全设备，但是有一天我用了反代，我几百个域名都指向同一个反代IP，我瞎了。

软件定义一切，物理架构会越来越简化，逻辑架构会越来越复杂。

我需要一个从域名知道后端真实服务器IP地址的工具。

在平时我们做应急处置时，看到一个自己IP，所有人对着这个IP很懵圈，他是谁，他从哪里来？他是干什么的？

所以我也需要一个输入一个IP，他相关资源可以浮现的工具。掌握不同权限的人会看到不同的内容，VMware管理员会知道他是一台虚拟机，反代管理员会知道他对外提供的URL，前几天正好访问过这个URL的人知道这个网站是用什么语言编写的，但是他们无法将这些拼在一起。

![](/images/2021/ipinsight/blind-and-elephant.jpg)

所以我写了些代码，每天定期从各个系统去抓取IP的所有数据，放到数据库内来查询。

写着写着，我发现可抓取的东西越来越多，多一个信息源，就多拨开一层迷雾。

所以IPInsight说简单就是，一个给数据中心管理员或者说安全人员使用的工具，他可以看到跟这个IP和IP关联背后服务器的相关信息。

如果要比对，就是校园网内的Shodan。

## 目前获取的内容

![](/images/2021/ipinsight/kibana-index-management.png)

- beian：备案的各个网站的项目名、URL、IP、简介、联系人等信息。通过备案系统吐出JSON格式的数据获取。
- dec-client：数据交换中心防火墙各个IP信息和备注。通过dec的Python脚本获取。
- nmap：nmap IP地址得到的端口开放和服务识别信息。
- nmap-from-evil：同nmap，从定义的非信任IP发起扫描。
- ns：DNS权威服务器的数据，包括域名CNAME的内容和A、AAAA数据。从DNS权威直接发送数据。
- nsfocus-rsas-vuln：获取nsfocus漏扫到的漏洞名称，威胁值。未找到API接口，模拟Web登录，启动任务，等待4个小时后模拟下载XML格式文件，分析并载入ES。
- phpipam：phpipam是开源的IP分配管理系统，获取IP分配的联系人和VLAN信息。从MySQL数据库直接获取。
- puppeteer：Headless Chrome获取到的网站的元数据。
- qzsec-device：堡垒机的资产信息。
- safedog-port：获取safedog轻量级Agent获取到的服务器端口开放情况。通过API获取。
- safedog-server：safedog轻量级Agent获取到的服务器信息。通过API获取。
- tracepath：使用tracepath命令追踪传输路径。
- vmaware-vm：获取VMWare虚拟化平台虚拟机所在的目录，虚拟机名字，IP地址，VMWare Tools获取到的信息，Custom Attributes等。通过pyvmomi SDK获取。
- vsb-website：网站群，URL，名称，新闻条数。通过PostgreSQL获取。
- webpro-website：网站群，URL，名称，新闻条数。通过MySQL获取。
- whatweb：使用whatweb获取到网站的元数据。
- wrd-wengine：wrd反向代理内配置信息，网站的回源地址，目录位置，连接状态，启用情况，访问策略等。通过API获取。
- zabbix：监控，IP，监控类型。通过API获取。
- zdns-network：zdns IP管理系统的数据，IP段分组，联系人信息，自定义字段。通过APi获取。
- goby...

## 有什么好处

从上面可以看到，几乎能想到的，我都收集了。然后所有这些数据，都用IP或URL串起来了。

![](/images/2021/ipinsight/tanghulu.jpg)

- **从白盒跟黑盒视角是不一样的**。互联网的诸如Shodan类似的平台，都是使用nmap扫描端口，然后对端口做深度的指纹识别，做漏洞扫描。而IPInsight，是主动将系统的部分信息暴露给统一的中心，数据更为准确和全面。当然，个性化对接编写的代码工作量也更大，**有点废代码**。
- 权限隔离。为了掌握这个IP的所有信息，你不需要相关不同系统的权限，即使你不是反代管理员，你也可以在这个系统查到反代的信息。
- 统计分析。
- 关联分析。
- 如果说备案和phpipam的IP分配是静态的话，静态的数据，一般过一段时间就会跟现网数据不同，而其他都是动态的，真实的，动态获取的数据，可以跟静态的比对，找出为何会不一致的原因。
- 这里，暂时还没有网络流量分析和HTTP协议分析的数据。

## 选择的ES

我几年前就开始在搞这方面的东西了，为什么选择了ELK？我最开始在Cassandra和MongoDB之间徘徊了很久，然后我选定了MongoDB，使用Flask写了一些代码，代码无比丑陋。直到有一天我听到有些人不屑地说：“网络空间测绘不就是xxx和Elasticsearch么？”啥，应该是用Elasticsearch？

于是我就开始尝试用ES，很快我就发现，Elastic Stack你天生就是来干这事的吧！时序数据库，适合不停的抓取数据，自带ip和iprange字段，自带Kibana分析器，可以做一些简单的分析，固定的，复杂的分析我再通过Flask来写，减少了非常多的代码量。

因为IPInsight经常要跑，全量跑一次可能要几个小时，跑完会有一堆数据，为了将这些数据关联在一次事件里面，我开始想了“批次”的概念。在每次启动时，生成一个批次，接下来所有跑的数据都带着这个批次。用了ES后，我做了个约定，每天早上1点开始各种跑，然后Kibana搜索以UTC 16:00，也就是Today来过滤数据，虽然这样子让我丧失了任意时间跑的灵活性，丧失了lastest的数据，但是代码量和架构也简单了非常多。

## 我所想到的一些场景

很多我还在挖掘，有好的想法可以告诉我。VPN的解析还没写。

- 应装尽装，查找存活的服务器没有安装某狗轻量级Agent的。群发邮件通知安装。
- 列出还没被反代的域名，建立进度条。
- 退“公网”还“内网”。对于已经被反代的服务器，可将公网IP释放，使用内网IP。或在防火墙上设置策略，等同内网。
- 列出IP资源和他的关联资源，同虚拟机目录，同网段，同域名等等。
- 列出违反最佳实践的服务器，比如没有安装VMware Tools，hostname未设置等等。一般可以认为，这些没做的人，需要更多的关心和爱护。
- ...

![](/images/2021/ipinsight/matrix-know-thyself-1999.jpg)
The Matrix(1999)

