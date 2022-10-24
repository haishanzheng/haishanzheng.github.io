---
title:  "JIRA。。。忘了bugzilla吧。"
date:   2005-11-03 12:50:57 +0800
---

我以前曾经推荐过bugzilla，而且在我以往的项目中起到了很大的作用，看着bug一个个在减少，确实很有成就感，这种成就感使得修改bug也变得不是那么枯燥。唯一一个美中不足的是，他不能和我们的讨论区，任务分配系统结合在一起。也就是使用bugzilla，我们还需要一个todo list，一个内部的交流论坛，虽然你可以把todo也当成bug来管理，但是这样使得整个bug管理混乱，也无法正常使用bugzilla的报表功能。不过这一切在我试用了JIRA成为了历史。  

当然，忘记任何东西都是有代价的，JIRA标准版在国内卖一万二，包括12个月的升级和技术支持，而bugzilla是免费的。  

JIRA在问题上做了分类，问题的类型有Bug，新特性，任务，增强功能，甚至可以再自定义，你可以自定义一个项目讨论的问题分类。就是这一个问题分类功能，从架构上已经比bugzilla高了一层。所以我的第一个问题就解决了，通过JIRA强大的过滤器（bugzilla过滤器也很强大），你可以清楚地浏览这些问题分类。  

在一些细的功能上，JIRA相对于bugzilla还有sub task，自定义问题字段，更强大的权限管理，问题的时效管理，可自定义的dashboard，屏幕截图，和cvs/svn的集成等等。安装上，JIRA也比bugzilla简单很多，bugzilla你必须检查很多依赖包，还必须自己建立数据库，而standalone的JIRA你只需下载一个jdk，一个rar包，解开，运行startup脚本就可以完成安装，standalone自带了一个webserver，一个hsql数据库。  

如果钱不是问题，忘记bugzilla吧。  

JIRA - Bug Tracking, issue tracking and project management software
http://www.atlassian.com/software/jira/  

