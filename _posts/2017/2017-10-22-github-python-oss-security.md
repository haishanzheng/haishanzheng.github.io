---
title:  "世界上最大的同性交友网站 && 蟒蛇 && 开源软件安全性"
date:   2017-10-22 09:58:09 +0800
---

前几天世界上最大的同性交友网站GitHub公布了[年度报告](https://octoverse.github.com/)，Python窜到第二位了。我不拘泥于某种编程语言，但是我越来越多的在用Python，图书馆i学堂周三推了个课程[一起来学习Python](https://mp.weixin.qq.com/s?__biz=MzA4NTA1MTgxNw==&mid=2651630550&idx=1&sn=41ed4283046545a19a50e9f55f9685f8&chksm=8425db10b35252064df17f85ab06c94688fac52297bc21e4f6d537407e2648189aede05d9b3f&scene=0#rd)，人数超过300人，不得不从三楼会议室转换到更大的5楼报告厅。如果遇到一个pythonic的人，他应该会七步成代码一段：

{% highlight python %}
meeting_room_name = "3楼区域研究资料中心" if number_of_participants < 300 else "5楼报告厅"
{% endhighlight %}

当然，据说有人是奔着讲师的颜值去的。对于Python学习，我会建议什么？我会建议你第一步看PEP8。

我几年前就在同性交友网站注册了账户，只是没贡献任何东西，接下来我可能把我的博客迁移到GitHub，因为我发现有无聊人想要评论的话，他可以给我提交Pull Request，达到我交友的目的？

- 我曾经说过：评论比文章更精彩。
- 我后来说过：垃圾评论猖獗，审查制度趋严，动态网站安全性堪忧，我已经关闭了评论。
- 我现在又说：来提交Pull Request吧。

中科大张焕杰在群里不停地安利Git，确实，不用Git的IT从业人员可能是不合格的。厦门大学也有为写代码的童鞋提供的[Git源代码托管中心](https://git.xmu.edu.cn)，版本控制，支持组，私有代码托管，只允许厦门大学教职工和学生注册使用，使用的是开源的GitLab。我自己在里面托管了3x个仓库。

但是Git越发达，开源软件安全问题也就越多。我Linux用得比较多，我发现我经常必须不停的update操作系统，有时候我想，Windows好像也挺好的。为了不重复造轮子，我一个项目会引用非常多的开源软件，每个开源软件又会依赖其他的开源软件。得益于包管理器，我不用去解决这些依赖问题，我只要一个命令即可安装一个包，然而如果不小心typo错误的包名，我可能就会被类似这种的[假的浏览器插件名](https://www.macrumors.com/2017/10/10/fake-chrome-extension-google-web-store/)攻击。

而且每个开源软件又有非常多的提交者，我觉得这一切就如同麦当劳的巨无霸，一个牛肉饼由上万只牛的肉搅碎并压制而成，一旦有一只疯牛病，就全部悲剧了。

摘自开源软件安全现状分析报告 http://www.freebuf.com/articles/terminal/149575.html ：

> 从2015年初到2017年初两年多时间中，360 代码卫士团队从GitHub、Sourceforge等代码托管网站和开源社区中选取了2228 个使用比较广泛的开源项目进行检测，涉及的开发语言包括C/C++/C#/Java等。检测代码总量257,835,574行，发现源代码缺陷2,626,352 个，所有检测项目的总体平均缺陷密度为10.19个/千行。

我在用着华为手机，先不吐槽华为应用商城下载不到小米计算器等友商的应用，甚至在P10出现闪存问题时下线Android Bench应用。由于没办法用Google Play商城，我要用的开源软件在各大应用商城都找不到，什么KeePassDroid、OwnCloud、FreeOTP，所以我安装了F-Droid。这可能也是一个坑，有时候想想，苹果好像也挺好的。

> F-Droid is a robot with a passion for Free and Open Source (FOSS) software on the Android platform. On this site you’ll find a repository of FOSS apps, along with an Android client to perform installations and updates, and news, reviews and other features covering all things Android and software-freedom related.
>
> F-Droid is operated by F-Droid Limited, a non-profit organisation registered in England (no. 8420676).

我所见过的事物，你们绝对无法置信。我目睹了莘莘学子的论文在WannaCry的肆虐下摧毁殆尽，我看着&lt;?php $_GET&#x5B;a&#x5D;($_GET&#x5B;b&#x5D;);?&gt;在404.php的黑暗中闪烁。所有这些时刻，终将流逝在时光中，一如眼泪消失在雨中。

![](/images/2017/blade_runner.jpg)

我们总有一天会被开源软件搞死，总有一天会被核弹氢弹什么蛋搞完蛋。

