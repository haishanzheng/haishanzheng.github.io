---
title:  "华住数据泄露，隐私这个词必将从词典消失"
date:   2018-08-30 15:28:09 +0800
---

华住旗下酒店5亿信息疑被泄，专家：或因华住程序员失误所致 https://www.cnbeta.com/articles/tech/761909.htm

## 背后的故事

华住说在核实调查，报案，那推测他们已经确认信息是他们的了，不知道是否还舍不得花38万把数据买回来验证，又不知道新的安全法买数据的人是不是也是犯法？

这个事情刚出来时我有点想不明白，一般的黑客套路不是应当找公司勒索，公关赚到一笔钱么。当然公司还是有风险的，只能是争取时间做好其他公关。把数据卖了只能赚一次性的钱。在暗网建个云服务按次收费细水长流都比这个好？所以我首次判断可能数据是假的，但是黑客又提供POC数据验证。

而且时间太快了，13号密码传输到GitHub，立刻就被脱库，十几天后直接公开。

后来想明白了，华住作为上市公司，还是可以从股票等渠道赚到钱的。这背后的故事，一定很精彩。

## 程序员的锅么

专家说，或因华住程序员失误所致。全能的程序员又背锅了。分析已经披露出来的信息，基本上全线纵深防御都失控了。靠谱的安全体系应当是考虑即使程序员被抢指着头，泄露了密码，数据也不会那么容易泄露的。

我们分析看看华住这次泄露的细节，如果截图是真实的话。

- 使用公开GitHub，舍不得买会员私有托管。不建自己的私有Gitlab。
- 使用公开的仓库，源代码被人审计，信息泄露。
- 密码传输到Git。敏感信息传入Git是不允许的，Git就像区块链，数据传入后是很难删除掉的。我没去研究如何干净删除Git的某个版本，因为我还没遇到过，如果已经传入的敏感信息密码是随机的，没有在其他地方使用的，更新即可。敏感信息不允许被传入版本控制系统，Log服务器。一般的机制是密码使用环境变量注入，或者写到配置文件，配置文件名放到.gitignore里，同时提供一个模板方便其他开发者拷贝快速建立。
- 数据库没有防火墙保护，放在公网。
- MySQL账户名是root，密码是弱密码123456。多个不同安全级别的数据库之间没有隔离。
- 数据大量泄露（53G+22G+66G=141G）没有被IDS等流量分析工具预警。

可以看出华住基本上没有任何安全防护，不从这里泄露，也一定可以从其他地方泄露。

## 安全是成本

**我们谈安全都是谈成本，愿意投入多少资源可以达到哪个安全级别**。我们谈安全还谈集中，集中才能降低成本。然而集中这个在酒店业有点难，我不是太了解酒店业IT系统，但是推测除了订房等可以通过OTA云服务器，很多的监控、门禁、消防、WIFI等操作都必须在酒店内放一台服务器。而且很多还是民宿，成本已经很低了，怎么再来谈安全。想要提高安全，可以从羊身上薅羊毛，住宿费百分多少直接提到安全专项经费里，这个短期内会增加成本，长期来看应该可以提升竞争力，等等，华住会破产么？其他酒店会开始关注IT系统安全么？我觉得不会，酒店业还有更多的卫生等成本需要考虑。

哪个酒店会广告说：保证开房数据不被泄露？不被泄露唯一的办法就是数据定期删除，真正在世界上消失。从备份删除，从Log删除。现在存储太便宜了，又有大数据分析的需求。我记得20年前某大学BBS系统硬盘空间不足了删信删用户删文章，这在现在看起来是很难想象的。从和尚事件得知短信可以从运营商处调取，保存50年，微信信息？应该也是参照这个标准吧。**如果你没有能力保护她，就放手吧**。匹夫无罪，怀璧其罪。

![](/images/2018/bhqz.jpg)

数据可以参照欧盟《一般数据保护条例》（GDPR）让用户主动删除或者定期清理，这需要对酒店业的IT系统做很大改变，我住的酒店不多，但是我还没看到哪个系统能让我登录上去查阅我的住宿记录的，更别说下载到本地后远程删除了。当然数据必须保留一段时间以接受监管部门的查阅。从滴滴顺风车事件来看，公众为了安全，希望有一个安全部门能够调阅任何人的信息。

## 请开始适应你的信息公开

社会在慢慢进化，这次数据泄露出来本来是个非常大的事件（1亿条手机号码、邮箱、身份证、家庭住址，数据集中化做得不错，嗯），然而不像疫苗那样子在朋友圈刷屏，大家都已经麻木了，仿佛数据不泄露才是新闻。

经济在发展，根据裙长理论(Hemline theory)，女人的裙子越来越短；科技在发展，以前两个人异地基本上就相当于老死不相往来了，现在看他的朋友圈你基本上还是感觉仿佛还在你身边。这是一个慢慢把我们信息往外泄露的进程，也是所有人慢慢适应隐私不再是隐私的过程。等最后所有人都麻木了，社会就进化完成了。而那些不适应的人就要被进化论优胜劣汰自然消失了。

我认为在目前科技水平发展下，社会最终是进化到所有人没有任何隐私，隐私这个词将从字典里消失。从信息系统大数据层面看你，就像你看地球上到处乱跑的其他动物一样一览无余。

![](/images/2018/blackmirror.jpg)

Black Mirror S04E06 Black Museum


