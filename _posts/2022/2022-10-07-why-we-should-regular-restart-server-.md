---
title:  "为什么你应该经常重启你的服务器"
date:   2022-10-07 10:40:01 +0800
---

如果注定要出问题，那就在大家都在上班的时候出吧！

如果服务器几个月不重启，那一定会有安全问题。当然对于那些连补丁都不敢打的，从安全的视角来看，谈论重启没有意义。

## 不可预期重启的危害

你**永远不知道**你的服务器会在何时重启，有可能

- 你自己重启
- 别人重启
- 系统更新定期的重启
- 虚拟化故障导致重启
- 系统蓝屏导致重启
- 掉电重启

你可以有些方法在操作系统层面阻止重启，但是你不能阻止所有重启。我遇到过太多因为重启引起的问题了。

- 突然掉电的重启是很可怕的，当然，如果只是用一些很标准的操作，还是可控的。操作系统和应用层面的容错率越来越高了。
- 重启后重要的配置没有保存导致失效，比如防火墙用命令行手写实时生效，但是没有写入持久化配置文件
- 重启后服务没有自动启动，很多外包公司从来不是写systemd的，启动后需要人工干预。
- 重启有自动启动，依赖关系没有处理好。比如NFS、数据库需要比Web先起来，而且有些依赖又不会自动重试。
- 物理机器，重启后挂了。很多机器，一直跑着，他可能就进入随遇平衡态了，一旦有个干扰可能就不正常了。**自行车，一停下来就要倒的**。物理机器hard reboot要跑自检，有启动阻力，自检会检查一些你运行时不会检查的东西。
- 哪天插着U盘拷贝东西，忘记拔了，重启后从U盘启动了。
- 重启后文件没有了。有些文件被删除了，但是不影响系统运行，因为已经加载到内存了，或者句柄还在，但是一旦你重启，这些都没有了。
- 虚拟机要启动发现启动不起来，虽然看起来虚拟机注册在平台里，但是底层文件已经被删除了。
- 病毒写入启动目录，重启后开始发作。

## 你应该在何时重启

- 做了重要配置后，一定要全量重启一下看看各类服务是否正常。不重启，会让你的变更导致的错误叠加，让你越发在下次重启需要解决更多的问题。
- 打了内核补丁，如果不重启操作系统有些是不生效的。如果只是一些服务或者运行的进程，一般更新完补丁会自动重启。Linux也有很多免重启的内核热补丁方案。
- 硬件需要更换
- 有一个跟重启相关的是出了故障，很多时候重启一下就好了，但是其实更推荐，如果对业务影响可控，应当在故障重现的时候找到问题，实在不行才考虑重启的方案。

## 我的业务很重要，SLA要求高，不能重启！！！？？？

SLA要求高跟服务器重启没有任何关系，**不是不重启的理由**。如果你的业务确实SLA要求非常高，你应当是使用负载均衡、滚动重启、减少服务预热时间等多个其他措施来保障，单个服务器的重启不应当影响高SLA要求。

不敢重启，实际上是暴露了你SLA保障方面的弱点。

在预期时间重启不应当被计入SLA，我以前玩魔兽世界，固定会在业界通用的周二补丁日（Patch Tuesday，MS是每个月第二周的周二）进行更新、维护。你如果算他的SLA是97%，跟体验是不一致的。

很多公司是打包成容器，每秒重启销毁几千个容器，却是他SLA高的保证。

## 各类操作系统对非预期重启的提醒

非预期重启问题这么多，我们也会有多个层面来保障减少非预期重启导致的问题，比如说数据库会有WAL预写日志来保护，电源方面有双电源，UPS的保护，甚至于在地震中利用电磁波的传播速度比横波纵波快的原理来进行预警让服务器安全关机。

各类操作系统也从各种方式进行预防和提醒。

Windows Server对于非预期重启，在下次管理员登录后会要求你记录重启的原因。

如果在服务器安装了Zabbix客户端，监控系统可以提醒你某台服务器机器进行了重启。

## 如何知道我的服务器是否需要重启

如果没有掌握正确的判断是否需要重启的方法，很多时候你打补丁等于白打。

### Windows Server

Windows Server有GUI界面的，会在右下角提醒你更新需要重启，并且让你安排重启时间段。

Windows Server Core等没有GUI界面的，可以通过注册表进行查询是否需要重启，这个查询可以写成PowerShell脚本重复检查。

```txt
HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing\RebootPending
HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate\Auto Update\RebootRequired
HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\PendingFileRenameOperations
```

### CentOS

相比Windows Server和Ubuntu，CentOS对于重启的提醒不是太明显，所以很多人会忽略了重启。

```sh
needs-restarting -r
```

输出类似

```txt
Core libraries or services have been updated:
  openssl-libs -> 1:1.0.2k-25.el7_9

Reboot is required to ensure that your system benefits from these updates.

More information:
https://access.redhat.com/solutions/27943
```

```sh
needs-restarting -s
```

会指出哪些服务需要重启，但是不需要重启操作系统。

### Ubuntu

Ubuntu会在你远程登录后显示

```txt
*** System restart required ***
```

也可以通过运行如下命令来查看

```txt
cat /var/run/reboot-required
```

如果文件不存在就不需要重启。

Ubuntu 2204 LTS自带了

```sh
needrestart
```

命令可以查看有哪些服务、容器、虚拟机、会话需要重启，并且有界面立刻让你选择是否重启。

## 如何知道我的服务器在何时重启过

### Windows

在GUI下，在“任务管理器”的“性能”“CPU”左下角，有“正常运行时间”。

在GUI下，在“事件查看器”里，“Windows日志”“系统”，“筛选当前日志”，输入“事件ID”为6005，可以看到各次重启时间段。

CLI下，有多重方法可以看。

```cmd
wmic path Win32_OperatingSystem get LastBootUpTime
```

```cmd
systeminfo | findstr "系统启动时间"
```

```powershell
(get-date) - (gcim Win32_OperatingSystem).LastBootUpTime
```

### Linux

```sh
uptime
```

```sh
last -x shutdown reboot
```

```sh
journalctl --list-boots
```

/var/log

## 客户端怎么重启？

PC机、笔记本、手机、家里路由器，建议也经常性重启下，让操作系统甩掉包袱，跑得更快一点吧！
