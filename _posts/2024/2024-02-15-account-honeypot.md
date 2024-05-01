---
title:  "业务系统不止要有安全中心，还要有反制中心"
date:   2024-02-15 18:10:33 +0800
---

2020年，我写过 [每个业务系统都应当有一个安全中心](/2020/11/25/every-informatization-system-must-implement-security-center.html) ，在里面，我提倡整合业务相关的安全设置，在一级菜单位置给安全留一个入口，就是安全中心。

我看的业务系统不多，我去年真的在某个网站群系统看到了安全中心，非常实用。

在这篇文章里，我将会又乱建议某些业务系统，在攻防演练或者日常检测预警中，不要一直把自己处于被动挨打的状态，是时候，起来反抗了。

## 想法的由来

我曾经分享过 高校网络安全攻防演练应试解析，姚政茂写了篇[解析](https://mp.weixin.qq.com/s/st7lw_5dyPUu5Ozc9qhBTA)。

在这次分享里，因为我认为攻防演练是大考，所以我模仿中小学等第制**瞎编**了一个ABCD的评价体系，接着基于这个评价体系**捏造**了3个维度进行了分析。为了怕时间不够，我又从另外一个角度**拼凑**了篇幅。

![](/images/2024/abcd.png)

在谈到“一个系统攻防演练的时候被扣分，是否就一定比其他没被扣分的系统更不安全？”，我举了个例子。因为我发现，有些系统，普通用户的账号密码被攻击队拿下了，然后攻击队穷尽一切办法，用标准渗透测试手法从SQL注入、文件上传、平行权限、越权所有手段都试过了一遍，还是找不到任何漏洞，而且拉到的数据，也就是这个用户自己的数据，攻击队浪费了好多时间，最后提交账号分悻悻走人，系统被扣了账号分。

比较典型的就是邮件系统。这种系统，是经过攻击队认证过的，无比安全的系统。这时候你如果根据考核体系对这个系统的责任单位进行通报，实际上是非常不公平的。其他没被摸到的系统，问题可能更多。

对这种系统，应当通报表扬而不是批评。

这种系统一般都是账户量比较大，系统自身可能已经做了密码防止暴力破解，双因素认证等等防护，然而因为用户自身的问题，导致密码被泄露出去。典型的还有VPN、统一身份认证等等。这些系统也是攻击队撕开防线的关键一步。然则攻击队一般只在于拿到账户，不会对系统自身做什么攻击尝试，只要这个系统不要太弱智。

优秀，但是独善其身就够了么？能不能走得再远一点？为什么我一直都被打，从来没有想过反抗呢？

## 蜜罐类账号

攻击队这么喜欢拿账号，那就慷慨地送他们一堆吧！

我对蜜罐类账户的定义是，由前面提到的入口账号类的系统，比如VPN、邮件、统一身份认证系统提供的一类虚假，用来定位的账号。

这些入口账号类的系统，可以有个模块，生成一些假的账号，对这些虚假账号进行全生命周期管理，生成，标记，投递，回收，销毁。这些虚假账号，密码是什么无所谓，但是在验证请求时，如果有人提交这个账户和密码来验证，立刻记录并报警。

或者是真实的账号，引入胁迫密码。胁迫密码是什么，就是你在家门口，被人用Q指着头，向你家密码锁输入的一段密码，他可以让你进门，也会自动悄悄报警。

投递有几个方法。

- 扔到GitHub这类公开的平台
- “不小心”写到培训手册里
- 投递到成员邮箱，桌面去
- 写到出错后出现的错误信息里
- **在你收到钓鱼邮件后，不要删除它，不要忽略它，大胆地从蜜罐类账号挑选一个，提交过去，主动中招，然后就等着攻击者来验证，以此来获得攻击者的IP或者其他溯源信息**

这样你未来就会有一个表，谁，从哪个渠道转化过来了，用了什么IP或者指纹信息来验证。

这种蜜罐类账号非常隐蔽，因为他跟业务系统足够紧密。如果你起个蜜罐邮件系统，孤零零一个邮件系统部署在那，没有任何意义，没人会去玩。而这个够真实，没有能力，怕被监管部门通报，你就返回密码错误给攻击者好了，有能力，你还可以再把攻击队往某些地方引。

以往蜜罐会混到安全域里，会寄生在服务器、HTTP、数据库蜜标等等，而这个，实际上是业务系统自己做了蜜罐功能。

## 检测到自身正在被攻击

要不要识别到自身正在被攻击？

这个确实不是业务系统应该考虑的。网络的分布式和分层的架构，很多事情可以在不同位置、不同层级来做。比如安全，因为成本，你可能只在某个地方集中做；为了纵深防御，你可能多个层都做；因为在扯皮，你可能偶尔这边做，偶尔那边做。

让业务不要识别被攻击，可以让业务更简单，专业的事情交由专业的类似外层WAF内层SIEM都可以做。但是有些地方，还不得不业务系统自己才能感知到。

举些例子。

### 有哪些是开发者更容易能感知到的攻击

**有些程序员无法正确区分错误和异常。**就算对于异常，他们也无法分清，不知道该失败重启还是清理环境继续。

很多业务系统对出错不处理，一个错误，只会在access.log或者error.log留下一条记录。只要系统不出现故障，不会有人主动去分析错误日志。

我以前遇到一个业务系统，每次遇到500错误，他都会将错误记录下来，写入数据库，并且在前端有个界面可以看到，由哪个用户引起的这个错误。

就这个事情其实很多业务系统都做不到。那个业务系统，借由这个功能，识别到了一次普通用户密码泄露引发的攻击尝试。每次成功的攻击背后，都可能伴随着大量导致程序出错的尝试。

而如果你的业务系统已经引入RASP了，那能漏到RASP的，应该是非常严重级别的告警了，应当直接触发“强制报告”。

还有一种非常常见的场景，比如项目管理系统，你输入合作者的学工号，或者只是姓名的一部分进去模糊搜索，业务系统就会非常贴心地将这个合作者的相关信息（姓名、身份证号、联系方式、院系、银行卡）啥的都帮你代填了。

这个功能你又去不掉，用户很喜欢这个功能。攻击者也很喜欢，写个程序，就可以把全部人信息都枚举下来。

为了防护，你可以部分打码、取得合作者授权同意、控制阈值等等方法。你就很烦，本来我给你设计的路线是这么走的，但是偏偏一些旁门左道的人，脑洞比谁都大，在你这里乱搞，让你加班多写了好多代码。

那就再多写一些，抓出来好了！

希望未来我能遇到这种带刺的业务系统。