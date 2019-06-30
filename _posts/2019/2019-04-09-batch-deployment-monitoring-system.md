---
title:  "一键部署Zabbix、Grafana、Icinga、SmokePing监控系统"
date:   2019-04-09 15:28:09 +0800
---

管理学上有一句名言，**If You Can't Measure It, You Can't Manage It**。对于监控的重要性我就不细说了，原先我们使用Nagios、Catti、SmokePing、Icinga。Prometheus、Grafana、collectd、ELK、Graylog2啥的乱78糟的也都在用，每个开源软件都有他的适用场景。最近是由于原先安装的Nagios操作系统版本太老了，而且Nagios都9102年了都没什么大更新，索性我重新做了升级，并且全程使用了自动化部署，将一键部署源代码放在GitHub上，

https://github.com/haishanzheng/CampusMonitor

欢迎试用，觉得好用，请Star，有意见或者建议欢迎写信给haishanzheng@sina.com。

## Why Them

为什么选择Zabbix、Grafana、Icinga、SmokePing组合？主要是对应信息中心的2个需求。

- 业务是否正常，涉及到网络交换机，网站和业务流程。
- 测量师生在多出口环境下对外访问互联网网站延迟。

对应第一个需求，由于Nagios太老了，所以换Zabbix。Zabbix功能非常强大，GUI、API配置，然而我还是忘不了Nagios的文本配置对于程序化生成配置文件的友好，所以也引入了Icinga，2套类似的系统并行。

测量延迟使用SmokePing，SmokePing使用Perl和RRD文件，技术上稍微落后，但是配置异常简单，能达到目的也行。当然最近某网也出了很帅的“此版本与60寸以上大屏幕更配哦”的基于现网流量时延分析升级版，SmokePing可以认为是主动的定向的测量。

引入Grafana是地质大学宋焘的一句话给我打开了新天地。确实，Grafana的界面更加酷炫，Zabbix更偏向内部系统，通过将数据传递给Grafana，做一些友好性处理，剔除技术细节，就可以做个类似 https://grafana.wikimedia.org 或者中科大张焕杰的 http://linux.ustc.edu.cn/ ，向公众开放可访问的界面，让用户也可以看到整个校园网、数据中心和业务正常情况，减少故障报修订单。当然，是不是真的敢对外公开这个以后再说了。。。

这是整体框架图。

![](/images/2019/monitor/architecture.png)

## 自动化部署有什么好处

可以看框架图，部署是非常复杂的，为了安全，Zabbix使用主从模式部署，从代理服务器部入一卡通专网和互联网云平台。SmokePing可使用树莓派部署后到处乱丢，没有自动化部署工作量是非常巨大的，而且使用了自动化部署，也可以在不同地方复制，让其他闲人也可以参与进来。

最简单的部署就需要6台服务器，我还引入了Vagrant，引入Vagrant的目的是建立服务器都自动化了。原先你可能需要在云平台开6台服务器，配置IP地址，写入inventory，然后跑Ansible脚本，加入Vagrant后，只要安装完Vagrant、VirtualBox，在项目目录内跑一个命令vagrant up，就会自动建立6台服务器，并自动跑Ansible部署完成。然后就可以在本机玩了，当然本机的内存要大一点。

GitHub的代码只供参考暂时，可能有错误，很多东西我也在摸索，我会根据反馈继续完善。

## 有什么坑

幸运的是坑我都踩了，以Zabbix支持中文为例，全网查找你可能会看到很多让你传Windows字体文件，替换或者更改PHP源代码，但是实际上Ubuntu的Zabbix包已经自带了字体切换功能。比如：

    apt-get install fonts-wqy-zenhei

    update-alternatives --install /usr/share/zabbix/fonts/graphfont.ttf zabbix-frontend-font /usr/share/fonts/truetype/wqy/wqy-zenhei.ttc 20

在Ansible里面就是这么一段话

![](/images/2019/monitor/ansible.png)

这才是最优雅的实现。

## 说重点，界面长什么样

如果你花10分钟安装完Vagrant、VirtualBox，又花了半个小时跑完vagrant up，你会得到什么？

![](/images/2019/monitor/virtualbox.png)

6个虚拟机，一台主控controller。

### Zabbix

![](/images/2019/monitor/zabbix-dashboard.png)

Zabbix主界面，最下面是目前问题的服务器或交换机。

![](/images/2019/monitor/zabbix-switch.png)

基于SNMP自发现交换机端口后的对端口出入流量的画图。

![](/images/2019/monitor/zabbix-screen.png)

Ping的结果。

![](/images/2019/monitor/zabbix-problem.png)

资产视图，总的资产和问题资产。

![](/images/2019/monitor/zabbix-inventory.png)

资产视图。

## Grafana

![](/images/2019/monitor/grafana-linux.png)

Grafana以Zabbix数据源做的图。某台安装了Agent的Linux的服务器性能图。

![](/images/2019/monitor/grafana-compare.png)

Grafana可以方便叠加Zabbix的指标。找出Ping时延最大的多台交换机。

## Icinga

![](/images/2019/monitor/icinga-host-summary.png)

Icinga总资产。

![](/images/2019/monitor/icinga-event.png)

问题出现的时间序列。

![](/images/2019/monitor/icinga-dashboard.png)

Icinga Dashboard。

## SmokePing

![](/images/2019/monitor/smokeping-siming.png)

SmokePing左边是监控目标。

![](/images/2019/monitor/smokeping-overlay-bbs.png)

对某个目标，叠加多个树莓派检测的结果，比较。

![](/images/2019/monitor/smokeping-curl-overlay.png)

![](/images/2019/monitor/smokeping-chart.png)

整体Top视图。

![](/images/2019/monitor/smokeping-bbs.png)

单个服务器的Ping时延和丢包率。
