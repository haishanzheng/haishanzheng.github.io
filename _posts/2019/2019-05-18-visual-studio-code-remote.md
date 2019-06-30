---
title:  "Visual Studio Code支持远程开发了"
date:   2019-05-18 15:28:09 +0800
---

[VS Code Remote 发布！开启远程开发新时代](http://mp.weixin.qq.com/s?__biz=MjM5NTE3NDgyMg==&mid=2650321268&idx=1&sn=d001c2e0a4f72c32477081eedd31c210&chksm=bef091bd898718ab2b293bedd2bd12c02997b2db4ce09a918ebd217e395321669841840e44e4&scene=21#wechat_redirect)

Visual Studio Code Remote 允许开发者将容器，远程计算机，或 Windows Subsystem for Linux (WSL) 作为完整的开发环境。你可以：

- >在部署相同的操作系统上进行开发，或者使用更大或更专业的硬件。
- >把开发环境作为沙箱，以避免影响本地计算机配置。
- >让新手轻松上手，让每个人都保持一致的开发环境。
- >使用原本在本地环境不可用的工具或运行时，或者管理它们的多个版本。
- >从多台不同的计算机访问现有的开发环境。
- >调试在其他位置（比如客户网站或云端）运行的应用程序。
- >在不同的远程开发环境之间快速切换，安全地进行更新，而不必担心影响本地计算机。

使用 Remote-SSH 扩展，你只需连接到 VM，安装必要的扩展（如 Python 插件），然后你就可以利用VS Code的所有强大功能，如 IntelliSense、代码跳转和调试，就像你在本地开发一样。所有以上的功能，并不需要在你的本地开发环境有源代码。通过 VS Code Remote，轻松连接上远程环境，在本地进行开发。

![](/images/2019/vscremotessh.png)

为什么我这么期待Visual Studio Code Remote的发布，因为他解决了我很多问题。先来说说历史。

## 历史

过了这个月，正式版发布了，这些就翻篇了，就像现在小朋友一出生就会使用触摸屏，他一定不知道十几年前他老爸拿着Palm OS的Treo 680安装现在已经忘记名字的地图软件在香港时代广场附近手动导航的蠢事。

我个人电脑使用Windows 10，我的大部分开发都在Linux服务器上。推荐好用的编辑器Visual Studio Code。

### 网络通路解决

最开始我是在服务器上开放Samba，开发机网上邻居开发。后来由于445端口被网络部门封了以后，我使用了Portproxy在开发机和服务器上搭建非445端口的Samba服务来解决， https://dog.xmu.edu.cn/2017/05/19/windows-network-neighborhood.html。再后来我重装Windows后，不想有太多依赖，使用了sshfs来建立到远程的连接，重装系统的意义 ，一个不便是有很多个映射的网络驱动器。

我还有一个方法是在本机安装VirtualBox，开发在本机，发布远程从Git pull。

还有一种方法是自动化部署在WSL里面，跑Ansible到远程服务器。

### 静态分析Linting、IntelliSense等

为了在开发机也可以做静态分析，我必须配置Python或者nodejs或PHP运行环境，所以我安装anaconda隔离Python版本和模块，安装pip，安装composer，在虚拟机和开发机同时安装相同的包，这导致我的开发机变得非常dirty。

### 调试

如果是Python远程调试，必须配置复杂的pyvsd。所以基本上不调试，或者靠logger、print调试。

## 现在

Visual Studio Code Remote发布以后，以上问题都不存在了，真正可以做到，不管在任何地方，只要安装了最基本的Visual Studio Code，就可以一致的开发了。

网络通路只要一个SSH隧道，所有的编程时运行环境都在远程服务器上，插件在远程服务器用户目录下面。非常类似X Window的架构，不同开发者连接相同的远程服务器会得到一致的IDE环境。调试运行全部在远程，本地更类似是一张Visual Studio Code的皮。说起这个其实去年发布的Live Share也有点类似，可以多人编程协作共同实时共享编辑同一个项目，但是没有带插件，也可以attach远程的debug session。

正式版支持要到月底，如果你现在要玩，必须安装Visual Studio Code Insiders，连接远程SSH后，会在服务器上个人文件夹安装vscode-server-linux，如果是Python，也会安装ptvsd，正如框架图里面介绍的，除了UI和Theme插件，其他插件都可以在远程跑，也就是你无需安装任何pip或者composer环境了，调试ptvsd也起在localhost，更加安全和方便，单步调试功能也都正常。由于是通过网络，速度有点慢，当然，可以使用VirtualBox加本地虚拟机或者docker或者WSL来。

因为Visual Studio Code本身也是基于Electron框架，**我想今后再进化，可能SSH隧道都不用了，VSC也不用安装了，起一个Web Server，用浏览器就可以对远程项目进行修改调试了**。Vagrant+VirtualBox+Ansible打包了开发测试运行环境，VSC Remote升级版打包开发IDE环境，未来新加入的开发人员只要跑一个命令，很快就可以在浏览器里面通过IntelliCode AI加持，机械愉快地开始敲起代码了。

