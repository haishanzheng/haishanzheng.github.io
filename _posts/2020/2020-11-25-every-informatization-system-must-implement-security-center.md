---
title:  "每个业务系统都应当有一个安全中心"
date:   2020-11-25 21:47:03 +0800
---

每个业务系统都应当有一个安全中心

对，每个业务系统都应当有一个安全中心

不安全的业务系统，送给你都不能要。纯粹是给自己添麻烦。

写程序谁都会，但是要安全地写程序，很难。安全，从来都是比懂技术更懂技术。业务系统，永远不要等着别人来保护，不要以为别人会很安全的使用你的系统，一个弱密码就可以搞得大家鸡飞狗跳的，你要从各个技术保障，再辅以管理措施，只有每个业务系统自己安全了，整体才能安全。

## 没有金刚钻，别揽瓷器活

高校信息办部门现在的发展趋势趋向于统筹经费，信息化项目归口后，很多二级部门信息化需求就会冒出来，反正不是花我部门的钱，又能为我工作提供便利，让我升职，谁不想申请些经费来买个业务系统来玩玩。这时候内部平衡不过来了，就会请利益无关的外来的专家来评审。专家一般会从建设的必要性，紧迫性出发考虑。未来我觉得还得看历史，谁做得安全，就把有限的经费资源投给谁。

监管部门从来不想背阻挠信息化进程的锅，二级部门要建系统，他们会笑着说，**我们鼓励你们建啊，只是，你要建的安全。**

你不是申请经费买个系统或者请学生开发就完了，你自己也要配套技术人员，安全管理人员。这里还有一些等保的规定动作要做你也看一下。

我见到过，没有安全防护能力的部门，通过各种层级协调后拿到大量敏感数据，转身后就拱手给了开发公司去泄露。

**“下线系统是为了保护你。”**能把把人家系统关闭了还说得这么好听的，也没有谁了。

搞出事后有些人就畏难了，做得越多，攻击面越大，错得越多。但是信息化总是要发展的吧。

> 网络安全和信息化是一体之两翼、驱动之双轮，必须统一谋划、统一部署、统一推进、统一实施。做好网络安全和信息化工作，要处理好安全和发展的关系，做到协调一致、齐头并进，以安全保发展、以发展促安全，努力建久安之势、成长治之业。
> 
> ————《在中央网络安全和信息化领导小组第一次会议上的讲话》（2014年2月27日），《人民日报》2014年2月28日

既然还是得干，怎么挑系统？

## 如何向等保三同步表达respect呢

等保三同步说到：同步规划，同步建设，同步使用。

安全关口要前移，安全要如影随形。

具体怎么做呢？在信息系统生命周期内，在项目立项论证之时，就要将安全要求写入论证报告，在招投标时，要求建设方有相关资质，要求有在其他有安全实施经验。在需求阶段，要归纳出“**安全需求**” ，实现在“安全中心”，并围绕业务系统自身的“安全中心”在上线后进行安全运营。

结果就是：**每个业务系统都应当有一个安全中心！**各个业务系统的安全中心就是全校整体“安全管理中心”一个分形的存在，是安全层层压实的体现。

安全中心，就是在研发已经知悉并解决了注入漏洞、XSS、CSRF、平行权限、访问控制、默认配置、供应链管理等用户无感的问题后，想要对外展示能力的需要。这个安全中心不应该是甲方要求的，不应该是系统部署人员想的，不应该是安全部门强制的，而是研发自发要做得，安全，如同天然就在这个业务系统的基因里面，并通过安全中心显性出来。

要选，就要选这样子的业务系统，对于我来说，我就不信，一个业务系统安全中心都有了，他的安全能差到哪里去？

## 安全中心长什么样

我们经常会在各个软件、业务系统内看到安全配置，但是藏得比较深，很多安全相关配置散落在各个不同的配置开关里，查找麻烦。

安全中心，就是在一级位置给安全留一个入口，这个入口可以放在最后面。在这个空间，整合所有安全相关配置。对于不同系统角色，都有一个安全中心，比如系统管理员，普通用户等等。而对于审计员，安全，本来就是中心。

再具体呢？系统配置方面，比如密码强度要求，双因素认证配置，第三方认证配置，防止密码暴力破解。等保要求的三员授权配置，系统管理员和审计员分开。备份，日志，加密，各种阈值限制等等等。

然后有一个动态的安全运营中心，比如监控，是一个链接，点击打开配套的监控系统；比如备份，能读取到自己的备份状态；比如日志，能知道自己扔给日志中心是否顺畅，有链接可以直接打开到日志中心；比如特权账号一页显示；比如能获知对自身的一些攻击记录；比如健康检查；比如对内部不正常状态的统计；比如异常登录日志；比如高危操作超水平线预警；比如上次第三方安全检查时间；比如显示自身部署资产架构，自身和系统和第三方组件版本信息和安全补丁等等。

用户方面，对账户的体检，配置的体检，登录日志聚合，各种“安全需求”集合等等。

再简单点，就是测评公司来干活，扔给他这个界面，你等保就过了。

最后

## 绿水青山就是金山银山

安全，有时候要学会克制。把隐私拿来换方便，类似人脸识别、Wi-Fi亿能钥匙等等的，从来都是伪创新。据说现在都得戴着头盔去看房了。

> 绿水青山和金山银山之间关系认识的三个阶段：第一个阶段是用绿水青山去换金山银山，不考虑或者很少考虑环境的承载能力，一味索取资源；第二个阶段是既要金山银山，但是也要保住绿水青山，这时候经济发展和资源匮乏、环境恶化之间的矛盾开始凸显出来；第三个阶段是认识到绿水青山可以源源不断地带来金山银山，绿水青山本身就是金山银山。这一论述的提出，从根本上更新了人们关于绿水青山无价的传统认识，打破了简单把发展和保护对立起来的思维束缚。

是不是跟我们信息化发展有点像？第一阶段开始一穷二白，大量兴建信息化系统，不管隐私不管安全，先污染一波再说。现在很多人还在第二阶段，等你突然领悟了看起来是束缚的安全可以带来信息化的价值，你就升华了。

真心希望看到未来每个应用都有一个安全中心，那些没有安全中心的，就慢慢让市场淘汰去吧。