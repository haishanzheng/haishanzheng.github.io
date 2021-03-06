---
title:  "运维的四种境界"
date:   2019-03-06 15:28:09 +0800
---

**运维的四种境界，你在哪一层？**

江南大学沈叶忠说：技术有几个层次，一是出了问题就打电话的；二是能明白，不被忽悠，关键时刻可以RESTART的那种，能分析出问题；三是能亲自动手靠自己搞定的。

这个确实非常形象地刻画了目前高校信息化部门在大量信息系统外包环境下运维的现状。只是我还想再升华提出一个层次，通过前期部署规范公司行为，提供完善的自检测脚本，维护的时候配合监控等手段，把问题消灭在出现之前。

华中科技大学的部署更严格，要求厂商提供部署文档，信息化部门自己部署，所有操作由厂商提供操作手册信息化部门实施。这可以杜绝很多包括目前我们经常遇到的厂商关闭系统防火墙测试导致系统被攻陷等问题，然而也会增加较大的工作量。

当然，要杜绝厂商关闭防火墙这个事，还有其他解决方法，比如通过批评教育。。。通过在堡垒机设置阻断命令，通过服务器外的防火墙，通过定期扫描端口等等，这个以后细谈。

我想说的是，在目前这个现状，信息化部门人手不足的情况下，如何正确管理或部署一个信息化系统。

我认为部署完一个信息化系统应当以所有服务器加入监控和备份作为部署的结点。

## 部署前期

业务的细节交由业务部门把握，只有业务部门才最懂他们的需求，虽然有时他们也不知道自己的需求，需求怎么落地，这个跟部署无关，技术都是可以实现的，衡量的就是成本。如果系统要部署了，找厂商拿到部署文档，精确识别出各个服务器不同角色的防火墙需求。防火墙规则可宽可严，简单点多个业务内部IP互相完全信任，严格点内部IP也限制到端口，以免一台被黑穿透到整个内网。这个最好业务上线前就调整清楚，否则等业务上线后要再加固就很麻烦。当然，从技术上来说，即使业务上线后，你要调整防火墙，也可以lsof -i，netstat -an，tcpdump，iptables drop log等多个手段来更柔性地加固，但是工作量会很大。

如果部署由信息化部门提供基础操作系统，最好从已经等保加固好的虚拟机克隆，或者使用Ansible脚本自动化部署基础环境。厂商的业务系统不一定有自动化部署能力，信息化部门能做的自动化包括安全加固，比如等保有十几个加固点，需要安装防病毒，安全补丁自动更新，监控Agent安装，防火墙配置，Log远程投递等等，也就止于此。

给厂商的手册。VPN账户密码，堡垒机账户密码，服务器账户密码，身份认证对接文档，数据库连接串，密码和操作文档分开，操作文档最好让厂商看过不用打电话问你任何一句话为原则撰写。

## 部署过程监控

部署过程和运维过程一样，应当让厂商从堡垒机操作，堡垒机有2个非常重要的功能，一个是审计，一个是学习，**我更要强调的是学习功能**，一个是甲方可以学习到厂商所有的操作，另外是甲方内部A/B角也可以互相学习。厂商的很多软件部署时非标准化部署的，有时候程序或者Log路径在哪都不知道，而堡垒机可以很快搜索到。堡垒机的其他好处，还是以后细谈吧。。。

部署方案也应当审过，有些厂商的操作是比较奇怪的，比如我们遇到某次某个系统加HTTPS，一般5分钟就可以部署好的，双机都可以不用停机，但是厂商绕了很大一圈要求停机几个小时，这种非常规的不合理的一定要提前就审出来调整方案。

服务器能分离要分离，架构要展开。我们以前有个系统，用了Redis缓存，Redis大家都知道，速度非常快，是不能密码保护来累赘的。部署的时候没有剥离，跟重要服务部署在同一台，Redis被攻陷拿到root，系统被用来挖矿，如果有做过剥离，则损失也可以控制在一定的范围。

部署过程的垃圾文件清理，什么.bak，数据库初始化文件，upgrade目录拉，有些部署人员不一定很处女座，这些留下都是为你后面找问题留下羁绊，定期要求清理。

## 用户自检测功能

什么是用户自检测功能？比如建行的“E路护航安全组件”，扫描你的系统环境告知你哪里需要修复。

某笔记本可以检测到用户硬盘可能损坏并打电话通知你根据你的IP地址判断你在某地某个酒店出差，备件已经到前台赶紧来更换避免了你的数据丢失风险，只是听起来有点怪。

像我们某系统，可能浏览器兼容性不好，可能某些ActiveX组件用户没有安装，可能PDF无法自动加载，这些都可以在用户打电话过来提供一个网页让用户先排查自己的问题，很多问题都可以在这里解决掉。

## 厂商系统提供的检测功能

我们某系统厂商做得很好，跑一个命令，会去十几台机器抓服务状态，并告知哪个服务不正常，这对系统管理员是非常友好的。

## 日志远程存储

部署完，要求厂商提供所有日志的路径，加上系统的日志，全部进入远程。一个好处是，日志的监控人员可以不需要有操作系统的权限（日志不要有敏感信息），可以关联查询。当然日志要说的东西也很多，高可用，多份存储，存180天，轮换，容错。低级一点存着就好了，高级一点跟监控联动。

还有一个非常重要的就是错误要清0，以前说过的。

## 监控

部署完成，所有可以监控的都加入监控系统，监控系统可以冗余。

监控他也是一个非常好的资产视图，以Zabbix为例，在外围你可以监控网络通断情况，服务器是否开机，端口是否正常，安装了Agent你可以知道CPU、内存、硬盘的使用情况，在更高层次把多个服务器组成一个业务组，可以查看业务的情况（比如做了高可用，2台互备，即时一台停机也可以不认为是问题），更进一步可以模拟业务操作，监控业务流程是否正常。当然监控的范围非常广，如果厂商支持，APM都可以做，这个我今后也会继续细谈。

自动化部署、部署文本化的Inventory文件也是一个很好的资产视图。

我前面说到的，在问题出现之前解决，比如安全，比如硬盘空间满，比如CPU负载跟过往不同，比如IO很高，这些都是问题发生之前的征兆，需要排查。

## 错误不能发生第二次

错误是一定会发生的，只是不能让他发生第二次时你还是无法快速定位出来，这时候就要引入编程的TDD理念和监控来处理了。每次发生一个错误，写一个测试脚本，加入监控，就累计了很多监控点了。

## 敢不敢自己解决问题

如果对业务系统不是非常有把握，建议不要，有时候越帮越忙。一些简单的重启倒是可以干。定位问题如果可以可介入，很多时候主要在于定位问题，找到问题了，解决很快的。

## 年报

**用数据说话**。如果再高级一点，可以定期出报表，比如根据堡垒机出运维次数和时间段报表，根据监控出SLA数字，根据issue tracking工具出统计，根据业务系统出管理员操作次数和业务人员参与度，和业务相关的报表一起提交。

比如微软正版化统计ROI，就可以通过激活日志，或者网络分析可能的使用量，最终出一个报告，为未来采购和运营方法调整做决策支持。

## 备份

备份？谈过了。https://dog.xmu.edu.cn/2017/02/04/backup.html

## 其他

备案，内部CMDB登记，是否纳入应用发布，WAF规则调整，上线前安全检查，漏扫，合同归档，源代码，文档收集。

## 举例说明

以我们某个系统为例，这个系统对用户来说很简单，就一个网址和一个APP，但是后端有15台服务器，包括前端负载均衡，PC版服务器，手机端服务器，文件共享NFS服务器，文档转换服务器，压测服务器，部分做了高可用，x2，部分搭建了测试环境，x2。

而且这个系统还依赖Oracle数据库，Oracle本身又是6台服务器组成的集群。

所有服务器都是虚拟机，依托在几台会漂移的物理宿主机上。

每个服务器上跑着不同的服务，比如Web、RPC、队列、NFS。。。

业务数据会从其他库同步数据，本身也对接统一身份认证。

配套数据库管理员、系统管理员、业务管理员、中间还要掺杂网络、虚拟化、安全人员，备份，日志，监控等不同角色的人。

所以这个系统，他的架构很清晰，然而依赖也非常重。表面上你看到业务不正常，要真正看到后端所有的依赖才能定位问题。有次我们在排查时发现某台服务器不通，折腾了一会儿，在几分钟内又发现不同业务的某个服务器也网络断线，就联想是否共同虚拟化集群有问题，如果这时候你没有监控，实际上是没法联想到的。之前即使你打电话给厂商，其实厂商帮不了你什么忙。

## 重建设，轻运营

这是通病，也是正常的，建设有新的东西出现，就像你购物收快递一样兴奋。但是孩子生出来了，辅导作业的时候就让你有想把他重新塞回去的念头。前几天我整理一个系统，重装系统更新到最新，框架更新到最新，去掉一些不必要的组件，自动化部署从Puppet换到Ansible，代码全部做了Linting，升级了统一身份认证，解决了一个安全隐患，整了半天，对用户完全透明无缝，如果要说有什么价值，只能是说系统更安全了，应对代码变更响应更快了，但是这些对外完全看不到。

这个其实都是在填坑。最完美的部署过程管理，由于人员技术精力工作量等原因，会留下一个个坑，随着业务系统部署越来越多，量变导致质变，**有一天突然会全部坍塌下来**。

最后，能用云的都用云吧。
