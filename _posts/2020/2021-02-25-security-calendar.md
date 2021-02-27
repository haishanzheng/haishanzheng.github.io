---
title:  "一次经典的规章制度活起来的实践"
date:   2021-02-27 16:47:01 +0800
---

前面我写了 [如何让规章制度活起来？](https://dog.xmu.edu.cn/2020/12/17/establish-regulations.html) ，正好我就遇到了这么一个考核，这个图是考核的工作要求，Word写得很专业，目录很清晰。

![](/images/2021/security-calendar/doc.png)

在这个工作要求里面，他是一个全年的工作，不是到年底突击一下就可以完成的，全年之间都有时间点要求，注重过程管理。

这是一个考核，对工作也很有指导意义。当然，他也配套一个信息系统，也可以看到任务要求和是否完成等内容，只是这个信息系统更关注结果，我在想**如何能通过某种方式更加直观的将这个工作在整年展示出来**。

基本的结构肯定是有一堆Checklist，完成一项打钩一项。因为有些工作是有固定Deadline的，有个零报告是每年不定时间持续一小段，这个用日历视图是最直观的了。

当然，如果你用云平台的日历，或者Kanban类似的工具，是可以直接拉一个这个日历出来，但是，这个数据无法共享，如果有人也要做这项工作，他只能知道方法怎么做，然后必须自己一个个建立。

所以我用React和Ant Design，整出了这么一个图。

![](/images/2021/security-calendar/all.png)

因为都是前端脚本，所以我托管到Github Pages，点击这个预览效果。 [https://dog.xmu.edu.cn/SecurityCalendar/](https://dog.xmu.edu.cn/SecurityCalendar/)

如果你也在做同样的这项工作，可以考虑GitHub [https://github.com/haishanzheng/SecurityCalendar](https://github.com/haishanzheng/SecurityCalendar) 下载源代码，在本地跑起来。可以扔到2K大屏，也可以简单就只打印出来。如果只是打印，会丧失当前时间的定位。

目前还只是静态的，数据不保存，所有需要更新均直接编辑源代码，比如完成一项工作，就改下isDone: true，需要更新的源代码和其他代码已经做了剥离。

![](/images/2021/security-calendar/src.png)

## 全年日历

日历使用rc-year-calendar，以橙色展示零报告时间段，将有期限要求的任务标注在日历上，移动到日期有任务展示。当天时间点暗红色显示。

![](/images/2021/security-calendar/cal.png)

## 有时间点要求的工作

有些工作需要提交支撑材料，材料有一些细则要求，关键点也都列了出来。作为小Checkbox方便打钩。

在有关键时间点的任务，显示截至到现在还有几天到期，利用luxon计算到现在的时间间隔，增加紧迫感。

![](/images/2021/security-calendar/duedate.png)

## 个性化时间点要求的工作

有些事情只要一年之内你有做即可，所以不列入具体时间点要求，通过编辑源代码，可以将时间点固化，推荐根据整个日历blueprint规划灵活的任务落在哪个时间段。

![](/images/2021/security-calendar/filtercateory.png)

## 再玩出花出来

因为现在只是展示，如果要保存数据，单人可以保存到localStorage，多人共享可以放到云平台免费的Key-value数据库里，甚至可以从既有的信息系统拿到API，直接更新状态。如果你有更好的展示效果，记得告诉我。

我一直希望工作上能有一个系统，按时间点来分配工作。特别是网络安全方面，事务繁多，三级等保测评到期，证书维保到期，应急演练，设备采购，年审，对服务器进行体检，漏扫，年初时间都定好，到时间了自动弹出来任务要求，傻傻做就好了。
