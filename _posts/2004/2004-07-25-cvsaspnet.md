---
title:  "用cvs来管理asp.net项目"
date:   2004-07-25 11:28:56 +0800
---

本来用vs，就应该使用sourcesafe来做为源代码管理工具，可是由于我的sourcesafe无法和vs.net集成，而且sourcesafe在我看来有2个很大的缺点，一是不支持远程访问，二是存储的文件格式为自有的格式，如果万一哪天系统崩溃，数据恢复是个难题。而cvs使用RCS，采用文本文件控制，想怎么搞都行。所以我们选择了cvs。  

涉及到开发工具：vs.net2003，nunit，nunit addins，wincvs。  

多项目结构：一个ns.Core项目，一个ns.CoreTest项目，一个ns.Stuff项目，一个ns.Web目录，  

由于这是个小项目，所以一个Core项目就足够，把compoents，enumerations，entities，controls全部往里面扔，大不了可以多建几个目录。  

Core里面包括了数据访问层，其实业务逻辑和数据访问都是混在同一个类中的。  

CoreTest为测试Core的项目。每个对应的类在CoreTest均有相应的测试类，采用把Test放在后面的命名规则。  

Stuff为一些资料项目，比如数据库建模的vsd和ddl文件，整个项目部署的readme.txt文件，还有一个template目录，这个等下会解释到。  

把目录结构搭建起来后，一个个加入cvs中，除了以下目录文件，把整个项目文件加入cvs管理。排除的目录为：bin，lib，obj，*.user，*.suo，web.config，ns.*.dll.config。  

因为没了web.config和ns.*.dll.config等文件，其他组员checkout后无法顺利编译调试，所以在Stuff项目加入一个template目录，目录里面存放web.config和ns.*.dll.config。  

在readme内写上checkout后必须手动拷贝的template文件，然后自行做修改。（一般是改改数据库链接字符串和文件上载目录），以后考虑写一个perl或者Python脚本来自动完成。  

这样每次update时会显示很多? bin NonCvs file，看着很讨厌，而且和加入的文件混在一起，混淆视听，所以考虑把他屏蔽掉，使用wincvs的Macros->Tcl->Add to .cvsignore，然后把.cvsignore加入cvs。整个世界清静了。  

目前项目正在摸索进行中，有意见或者建议尽管提咯。  


## 评论

*****
**匿名** 在 *2004-08-25 20:44:54* 说: 版主啊.我是.NET开发的.一直使用VSS的.因为在局域网里还算可以.习惯了.
现在想使用远程.进行远程的源代码管理,因为几个朋友在不同地区.想一起开发系统.
听说CVS可以实现.

目前我对CVS不熟.不知道怎么学.能不能介绍一下.

我们都使用window平台..

我的MSN: tintown_liu@hotmail.com 希望能帮帮忙啊.

*****
**郑海山** 在 *2004-08-06 16:19:56* 说: 本科毕业了，在网络中心工作。。。你也是计算机系di？

*****
**匿名** 在 *2004-08-04 09:35:45* 说: 竟然是校友啊，，楼主现在是本科还是研究生？

http://tommy5.net tommy5.net

*****
**郑海山** 在 *2004-07-26 19:15:03* 说: 一样吧，反正有版本控制即可。你使用linux，可能那些脚本更好写一点。

*****
**匿名** 在 *2004-07-26 00:31:03* 说: 我们现在没用wincvs，而是采用ssh远程登录+local cvs＋samba的方法。
不知你有没有兴趣，谈一下你们的方案比我的方案的优点？


