---
title:  "一个星期没有写新文章了"
date:   2003-10-04 20:25:08 +0800
---

已经一个星期了，最近在赶一个非常急的c语言小项目，所以很久都没有查看FeedDemon和blog了，今天终于告了一小段落，早上看了场电影，晚上就上来写一些文章。在这个c语言项目进行当中我可能会陆陆续续写一些小心得，如果你有其他经验要分享或者要请教，可以qq和我联系咯:)  

这个c语言小项目实际上是写一个界面，一个用来配置路由信息的界面。系统使用uClinux，运行在单片机上，由于单片机上ROM空间太小，所以画窗体等东东都得自己写代码。这个界面有点类似BBS，而我维护开发过厦大多个BBS（f8bbs，cs3bbs，xmuBBS），所以Windwood http://windwood.xmu.edu.cn/ 找到了我。  

其实我不是c语言高手，hwTony http://blogs.xmu.edu.cn/wayne/ 才是。对于我们编程人员来说，掌握的语言太多也不是好事:(写程序时语法规则就会混淆起来。我本身是Pascal出家，所以我写Pascal根本无需其他工具。而其他PHP，ASP，PERL，c等等我都必须有帮助文件才可以写下去，或者是自己以往写过的代码范例。对于ASP，关键字首字母要大写，对于c，变量名全部小写，中间以_格开，ASP，ElseIf是连在一起的，perl，字符串函数是strstr, lindex, strtok，Pascal是Pos, Copy。。。如果没有帮助文件，这是一个灾难。那天我在漳州要写一个小程序，在那边没有开发环境，我办公室的机器有防火墙，根本无法到我自己的机器上找代码。只有一个文本编辑器，好像已经够了？我先打算用PERL来写，可是愣了半天，愣是一个字都写不出来，于是打算用PHP，还是写的迷迷糊糊。后来回来办公室，才几十分钟给搞定了。  

所以像我这样的很多人就借口说，编程最重要的是编程思想，只要你懂了编程之道，知道多个类似功能需要用函数，一个函数只能做一件事情，注释要比代码多，不要多个地方更改一个全局变量，这样，不管一天出一种编程语言，只要给你帮助文件，你就可以在半天之内熟悉他，然后开始工作。当然，这是很理想的情况。  

不管怎么说，我们3个人就这样开工了，我做过某些项目，有项目开发经验，负责和hwTony商量搭起整个程序的frame，写一些跟主体功能耦合度较低的函数。hwTony对c语言很熟悉，能快速编码，负责c代码的技巧应用和写核心流程，技巧应用？什么概念？我编的名词，就是比如我们在讨论如何解决某个问题时，我说，我希望代码能写成xxx，c语言要如何做？能不能做到？然后hwTony想了0.01秒，就立刻提出一个方案。Windwood就负责客户联系和项目周边的事情。  

毫不夸张得说，我和hwTony的编程能力在厦大都是数九数十的，而且我们都有一个共同点是：过分注重编码细节，这是另一个灾难。我有时候都觉得我是不是太无聊了，我已经陷入了编程的一个怪圈，类似某个地方一定要放一个空格，if语句不管有多少子句都要用{}括起来，这个函数命名必须使用动词+名次这些可有可无的细节上。  

还有就是选择，举个例子，比如说这次界面，里面有4个功能差不多的窗口，我们有2种选择：1. 利用所有能利用的技巧，写出一个公用模块，这样4个窗口可能只需1个大函数和4行程序就可以完成。这样开始花的时间就比较多，但是以后10、100个窗口对我们来说都只有1个。或许开始花的时间很多，后来证明了这样是无法实现的。2. 分别写不同的4个窗口函数。这样时间花费非常少，快速而有效的代码，但是如果以后如果要扩展，就要不停得复制粘贴代码（复制粘贴代码这是编程的大忌）。可怜的是我们知道这2种区别，但是不知道如何选择。  

就这样一路磕磕碰碰下来了。。。  

