---
title:  "推荐PowerQuest的V2i Protector v2.0 Server Edition"
date:   2003-09-07 14:54:51 +0800
---

就我安装一台新机器的步骤一般是，把硬盘分为系统盘C和数据盘D，然后把安装程序都放在D盘，接着在不联网的情况下开始安装机器，等系统、补丁、常用软件都安全好了，再安装一个pcanywhere，就可以接上网线，回到自己的办公桌，泡上一杯咖啡通过远程控制安装维护机器了。安装机器是一件很简单又很浪费时间的事，所以我每次都把安装完的系统做一个备份。如果哪天系统出错，无法启动或者什么，覆盖一下就ok了。  

以前是用PowerQuest的DriverImage备份系统，这是一个非常方便的软件，支持NTFS，可以用online备份某个盘，如果备份目标是正在使用的系统盘，比如说C盘，DriverImage会自动重起并虚拟一个软盘引导系统做好备份再重起。这期间都不用你点击任何按钮，所以你完全可以在远程启动备份然后过10分钟再去连接，备份就已经做好了。不过昨天我在安装了一个 新世纪网络课程建设工程网络教学平台 机器时，安装的是Win2k Server，发现DriverImage无法安装在Server版本上，很是奇怪，不知道Server和pro的机制是否有不同？那NTSwitch http://www.southcn.com/it/itschool/200203271138.htm 是不是就是一个骗人的东西？  

最后在kason的提示下找到了PowerQuest的V2i Protector v2.0 Server Edition http://www.powerquest.com/v2i/protector/ ，看介绍  

_  
PowerQuest V2i Protector 可以大幅缩短服务器备份及复原时间,为用户提供一个全面的灾难复原方案，可节省大量时间和金钱，并支持持续性服务器备份、快速及时点(point-in-time)系统及数据回复!  

为全球商业客户提供周期性自动储存服务方案的供应商PowerQuest公司，宣布推出碟对碟备份及灾难复原方案PowerQuest V2i Protector 。 PowerQuest V2i Protector兼容Windows 2000及Windows 2000 Advanced Server，帮助客户维持数据及系统备份的整全性，并支持PowerQuest公司的周期性自动储存服务管理(Storage Lifecycle Automation Management, SLAM)架构。  

PowerQuest V2i Protector能够把备份镜像文件存放在近端或远端的SAN或NAS，亦能够轻易地载入及复原镜像文件内个别系统及数据档案。这种特有的档案浏览功能使管理人员能够快速检查备份内容，以及在复原档案前进行病毒扫描等工作。  

PowerQuest V2i Protector利用高速磁碟技术与虚拟容体影像功能的配合，可于极短时间内建立及时点备份影像并将之储存于中央系统，以便在整个储存服务周期内使用。  

_  
好像是一个功能非常强大的软件，非但支持online备份，而且可以备份到remote host，并且可以集中备份。不过我不需要这么多功能，还是先试用吧。安装一切正常，一路Next，期间还自动安装了.net Framework。运行起来，选择C盘，选择Image保存的目标盘，点击开始，10分钟后，备份就完成了，连重起都不用。。。，使用他自带的Backup Image Browser，好像是备份完整了。真是方便。  

做一个好的备份应用，就得把备份的数据恢复一下，也就是演习~~看看会不会出现问题，否则真正出现问题时就晚了。由于这台机器不是非常重要的机器，所以我也就没有试用恢复。如果你要使用在非常重要的应用上，一定要试着恢复一下咯，不要怪我没提醒你。。。  

总结：  
如果你是用win98，或者用FAT分区，大可以用Symantec的ghost，地球人都知道。  
如果你用Win2kPro，并且使用NTFS分区，则可以使用PowerQuest的DriverImage来备份系统。用PowerQuest的Partition Magic来重新分区。  
如果你用Win2kSrv，则应该用PowerQuest的V2I Protector 2.0 Server来备份系统，用PowerQuest的Server Magic来重新分区。Server Magic自带的Server Image好像也可以备份系统，不过由于找不到安装文件，所以没有测试，如果你测试了，请告诉我咯。如果你有更好更方便的软件，也要记得告诉我咯。  

如果你需要以上软件的可用版本，可以联系我:)  


## 评论

*****
**匿名** 在 *2004-05-01 15:35:50* 说: 我需要远程管理多台设备的备份，
但都未能成功，
确实依照帮助文件里面的说明做了，
但都会遇到“RPC服务不可用”
或者“没有足够权限”。
我都有点怀疑这个软件是否真的能实现
远程操作这项功能了：（

*****
**匿名** 在 *2004-02-25 16:17:27* 说: 你好，我需要以上软件的可用版本，能否发给我？谢谢！
我的联系方式是：
liqin@routon.com

*****
**郑海山** 在 *2003-11-06 14:11:36* 说: 就是原始安装文件吧。。。


*****
**匿名** 在 *2003-11-05 05:16:17* 说: 请问恢复当前盘时需要product installation cd怎么找啊，谢谢

*****
**匿名** 在 *2003-09-09 23:34:49* 说: 下载中...

*****
**匿名** 在 *2003-09-08 23:10:23* 说: Ghost 2003也可以识别NTFS格式啊。

