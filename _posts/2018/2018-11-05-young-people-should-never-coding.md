---
title:  "年轻人千万不要写代码，基于OWASP Top 10的代码安全"
date:   2018-11-05 15:28:09 +0800
---

今天去跟原来的房东清押金，胡建老太太说年轻人千万不要写代码不要读博，我擦真是真理啊！后来弄明白原来表达的是不要吸大麻不要赌博。。。

段子来自网上，但是我觉得怎么说都是我擦真理啊。码农也是一种易学难精的职业，一个团队输出的代码安全质量是以团队里面安全技能最低的那个程序员决定的，而且就这个程序员的破坏力是巨大的，就跟一个加班到浑浑噩噩的运维不小心

    rm -rf /空格var/log/apache2/access.log.15

一样。

再举个例子，大学校的信息化系统都喜欢定制，这个定制就是最大的问题，一个是拖累了项目进度，使得跟上大版本更新变得困难，而且很多定制是新手程序员做的。一些大公司中了一个大标，以这个经费基于公司原来安全的代码架构和产品找了一些新手程序员来做二开，同时也引来了巨大的安全风险。

下面我会基于OWASP Top 10 2017做个介绍、举一些互联网上已经犯过的例子和如何预防。

当然锅不是都程序员来背的，即使你写出有SQL注入漏洞的程序，我可以在前面挂WAF，黑客也黑不进来；即使你允许用户上传木马，我也可以限制目录不可执行让它变成困兽；即使你有XSS，我加个CSP；即使你没有对管理员权限进行判断，我也可以在应用交付对管理员链接加限制；即使你没有预防SSRF，我也可以在DMZ区限制服务器对内网的访问。**如果程序员不写出这些魑魅魍魉妖魔，我们搞安全的运维就可以统统失业了**。

程序员也不要假定会有搞安全的Guardian Angel在保护你，能保护你是他的能力，不能也是你自己的事情。即使你要假定，也应当假定前面所有保护都失效的情况下你如何保护自己写的代码。就像我们写一个函数，我们不去认为调用者一定会为我们做好所有参数的检查和判断，在函数内一定要自己再做好所有判断，你的函数应该是高内聚，健壮的。

当然代码的安全问题不仅仅是以下这些问题，这些只是比较常见的Top几，还有一些猥琐的方法是你无法预见的，**你只管努力，剩下的交给天意**。本贴可以跟《软件开发最佳实践：代码安全我们需要打什么疫苗》一起服用效果更佳。

以下内容有些拷贝自OWASP的中文翻译。

## OWASP

> OWASP是一个开源的、非盈利的全球性安全组织，致力于应用软件的安全研究。使命是使应用软件更加安全，使企业和组织能够对应用安全风险作出更清晰的决策。目前OWASP全球拥有220个分部近六万名会员，共同推动了安全标准、安全测试工具、安全指导手册等应用安全技术的发展。OWASP被视为web应用安全领域的权威参考。https://www.owasp.org/index.php/Category:OWASP_Project 有多达200个项目。

## 基本原则

- 对于任何用户的输入0信任。这有可能是从表单提交的，URL里的，上载的，Header里面的，Cookie的。
- 除了客户端JavaScript验证外，服务端必须再验证一次。客户端验证出发点是用户友好性。跟你去政府部门办事一样，你一次次去提交材料每次人家都会告诉你少了什么其他材料，类似浙江省最多跑一次改革办公室，其实就跟JavaScript验证一样，但是程序上JavaScript验证是可以绕过的。所以服务器必须再验证一次，有些框架可以提供在服务器端写代码就可以在客户端做同样的验证，可以采用。
- 有些不是代码的问题，是部署的问题，代码等保三级不代表部署也三级。但是目前DevOps部署也使用代码完成，所以还是代码的问题。

## OWASP A1:2017 – 注入

- 将不受信任的数据作为命令或查询的一部分发送到解析器时，会产生诸如SQL注入、NoSQL注入、OS注入和LDAP注入的注入缺陷。攻击者的恶意数据可以诱使解析器在没有适当授权的情况下执行非预期命令或访问数据。
- 是否拼接SQL/ORM/LDAP语句。
- querystr = “SELECT * FROM accounts WHERE id = “ + request.GET.get(‘id', ‘-1’)
- Query HQLQuery = session.createQuery("FROM accounts WHERE ID = '" + request.getParameter("id") + “’”);
- String ldapQueryStr = String.format("(&(uid=%s)(userPassword=%s))", username, password);
- 形如以上的代码都是不安全的。过滤或转义”/’、白名单、黑名单检测稍微安全一点，但是不是治本的方法。我们不应当去修改用户的任何输入，而且基于名单的需要经常调整策略。
- 正确的做法应当是使用参数化查询，或者使用框架提供的功能，框架会帮你做过滤。

## OWASP A2:2017 – 失效的身份认证

- 通常，通过恶意使用应用程序的身份认证和会话管理功能，攻击者能够破译密码、密钥或会话令牌，或者利用其它开发缺陷来暂时性或永久性冒充其他用户的身份。
- 有没有防止密码暴力破解
- 出错信息提示是否显示用户名是否存在？密码是否错误？对用户的显示有时候可以模糊一点，就告诉用户“用户名不存在或者密码错误”，多说无益。
- 密码是否加盐多次Hash保存，有没有帮用户检测弱密码？
- 有没有双因素认证，实现OTP成本很低，一个早上可以完成。
- 忘记密码逻辑是否正确。比如最近很火的《网络迷踪》电影一样，

Spoiler Alert

Spoiler Alert

Spoiler Alert

- 父亲要去登录女儿的Facebook，但是没有密码，于是使用取回密码，验证码发送到一个Gmail邮箱，他又没有Gmail的密码，于是又取回密码，验证码发送到一个Yahoo邮箱，终于这个Yahoo邮箱是他控制的，于是顺利登陆了Facebook。我们一般建议用户取回密码的邮箱是不常用的邮箱，这个邮箱是你的fail-safe。如果可以，学校的系统应当不允许用户使用学校的邮箱来忘记密码。
- 登录会话超时Cookie设置有效期多久。避免使用永久性的“记住我”功能。可以查看我写过的公众号文章《从运营商拿到你的Cookie究竟有多可怕》。

## OWASP A3:2017 – 敏感数据泄露

- 许多Web应用程序和API都无法正确保护敏感数据，例如：财务数据、医疗数据。攻击者可以通过窃取或修改未加密的数据来实施身份盗窃等犯罪行为。未加密的敏感数据容易受到破坏，因此，我们需要对敏感数据加密，这些数据包括：传输过程中的数据、存储的数据以及浏览器的交互数据。
- 敏感数据是否被记录到Log？是否有中间缓存？某程程序员调试将部分用户的信用卡账户信息存到Log文件被人下载，导致信息泄露。
- 有没有防止暴力破解：某科研系统在添加科研合作者的时候可以简单输入合作者的工号，就可以获取对应人的姓名、院系和出生年月等，极大方便了填报，然而也会造成黑客可以写个程序枚举获取全校所有人的相关信息。这种情况应当限制查询的字段，做到精确比对，返回结果应当限制个数，或者针对多次返回进行预警和阻止。
- 分享等是否默认限定过期时间？
- 密码是否提交到源代码版本控制系统？密码应当使用类似config.php.template等放入版本控制，或者使用环境变量来控制。某住事件，据说是程序员在GitHub内提交了数据库的密码导致信息泄露。《华住数据泄露，隐私这个词必将从词典消失》。GitHub2018史上最大更新，增加Token扫描功能。如果GitHub愿意为这个出一个功能，说明这个问题实在太普遍了。
- 传输是否使用HTTPS。HTTPS应当完美部署。《继续说IPv6、HTTPS、HTTP/2，系列完结吧》
- 对开发者透明的数据库加密保存不靠谱，因为黑客入侵代码后，也是以开发者的权限执行操作的，所以加密对黑客也透明了。所以无需对整个数据库加密，只对特殊字段加密，在使用时解密。

## OWASP A4:2017 – XML 外部实体（XXE）

- 许多较早的或配置错误的XML处理器解析了XML文件中的外部实体引用。攻击者可以利用外部实体窃取使用URI文件处理器的内部文件和共享文件、监听内部扫描端口、执行远程代码和实施拒绝服务攻击。
- 如果输入接收XML文件（RSS），注意XXE，比较不常见，一般在Office预览里。禁止外部实体引用。

        <?xml version=“1.0”?>
        <!DOCTYPE a [
                <!ENTITY b SYSTEM “file:///etc/password”>
        ]>
        <c>&b;</c>


## OWASP A5:2017 –失效的访问控制

- 未对通过身份验证的用户实施恰当的访问控制。攻击者可以利用这些缺陷访问未经授权的功能或数据，例如：访问其他用户的帐户、查看敏感文件、修改其他用户的数据、更改访问权限等。
- 平行权限，/article_edit.php?id=XXX，通过传入不同的ID就可以改别人的文章、看别人的照片。
- /admin_edit_article?id=XXX，猜测到管理员链接后可以直接行使管理员权限。
- 每个controller函数的第一段代码应当是做各种判断。资源是否存在，是否属于该用户，是这个用户的但是他是否有权限？是否每个函数都有判断权限？权限是否写入Cookie？
- 权限如果是配置的，是否只是隐藏菜单而没有真正对函数进行控制。十几年前我们破解PC程序有些就是这样子的，可能管理员账户登录后看到十几个菜单，普通用户登录后只能看到几个菜单，然而你去反编译Exe，或者不需要只去修改资源文件，你就可以控制对菜单项的显示和隐藏，也就有了管理员的全部权限。
- 用户上载的文件是否可以直接下载？/uploads/2018/07/22/1532254294.doc。如果没有启用URL Rewite的情况下，这种一般是没有保护的。你可能感觉后面一串1532254294好像比较长，黑客枚举时间比较久，然而这个数字实际上是时间戳，前面一段2018/07/22生成的153225xxxx都是不变的，所以黑客的枚举空间也就比较小了。
- 上载文件的保护应当：使用GUID重命名文件名，稍微安全一点，但会被某雷或中间人MITM等缓存。最安全的做法是放到Web目录以外，或者S3/NFS/FastDFS/Minio内，前端使用ID来获取文件，并在获取之前判断权限。当然最后真正返回有可能应当让Web Server来返回，比如Apache2和Nginx的x_sendfile模块，让Web Server来帮你处理资源消耗和断点续传等问题。
- 上传文件的先审后看，比对文章的先审后发。举个例子，一般我们一个网站允许非管理员账户发布新闻，我们会做先审后发，用户发布的信息经过审核才显示在网站上，然而很多上传文件没有做这个保护。比如一个网站允许用户注册，上传头像，黑客完全可以注册一个账户，上传一张恶意照片，然后在浏览器直接打开这个照片，虽然黑客注册完他的信息没有审核不会显示在网站列表被人查看到，但是上传文件没有保护，黑客可以截图对外宣称他黑了这个网站。不明真相的用户看到一张恶意图片，浏览器地址链接确实是你的，去访问也是可以访问的，他就会以为真的是你的网站被黑了。
- 对上传的文件进行病毒查杀，恶意代码检测。虽然上传的文件有问题不一定会导致你的网站被黑，但是会对下载这些文件的用户造成困扰，如果他们没有防病毒等一些软件保护，可能下载文件后就会中毒。

## OWASP A6:2017 –安全配置错误

- 安全配置错误是最常见的安全问题，这通常是由于不安全的默认配置、不完整的临时配置、开源云存储、错误的HTTP 标头配置以及包含敏感信息的详细错误信息所造成的。因此，我们不仅需要对所有的操作系统、框架、库和应用程序进行安全配置，而且必须及时修补和升级它们。
- 引入的第三方类库的默认权限是否禁用？测试/demo代码是否删除或者设置不可执行。以前某xxxeditor，示例代码有很多ASP、PHP、JSP等服务器端代码，是为了你更好写程序参考和测试的，如果没有删除这些，黑客有可能直接访问这些文件，对你的服务器进行破坏。
- Debug是否关闭。
- Web服务器是否允许列出文件。这个实际上不是个大问题，但是很多漏洞扫描工具会认为这是一个风险，上级部门会要求你关闭，可能就先自行关闭，免得解释麻烦。
- 怎么防：配置核查，自动化部署，最小安装原则。自动化部署不是解决你的安全问题，是解决你遇到安全问题后如何更快更新安全漏洞的问题。

## OWASP A7:2017 –跨站脚本（XSS）

- 当应用程序的网页中包含不受信任的、未经恰当验证或转义的数据时，或者使用可以创建HTML或JavaScript 的浏览器API 更新现有的网页时，就会出现XSS 缺陷。XSS 让攻击者能够在受害者的浏览器中执行脚本，并劫持用户会话、破坏网站或将用户重定向到恶意站点。
- Username is: \<?php = username ?>，HTML输出是否转义编码？
- 框架有提供默认转义编码，不转义编码需要显式调用类似 username | safe 的语句，这会提醒你更认真去考虑你要输出的内容是否有普通用户可以控制的内容。
- 是否实施内容安全策略（Content Security Policy，CSP），CSP为独立的安全层，致力于解决XSS，一般出现在有XSS的代码已经完成没法修改或者找不到人修改了，这时候只要Web Server配置好，浏览器执行。指定哪些外部资源（script-src,object-src,css-src,frame-ancestors,img-src,connect-src）可以加载和执行就可以预防一些XSS。

## OWASP A8:2017 –不安全的反序列化

- 不安全的反序列化会导致远程代码执行。即使反序列化缺陷不会导致远程代码执行，攻击者也可以利用它们来进行重播、注入和特权升级等攻击。
- 序列化是指将一个对象使用字符串来保存在多个环节内传递。boxing/unboxing。比如邮件的附件，也是采取编码成字符串来传递的，因为邮件协议只允许传输文本字符串。但是邮件是个附件，而如果你传递的是一个对象，这个对象有属性和一些方法，如果你允许黑客修改这个对象，黑客可以加入或修改一些方法，在你目的地unboxing的时候执行了这些方法，有可能就被黑了。
- 反序列化一般问题较小，利用很难。
- 使用数字签名检查boxing和unboxing之间的完整性。

## OWASP A9:2017 –使用含有已知漏洞的组件

- 组件（例如：库、框架和其他软件模块）拥有和应用程序相同的权限。如果应用程序中含有已知漏洞的组件被攻击者利用，可能会造成严重的数据丢失或服务器被接管。
- 类似A6:2017 –安全配置错误。
- 组件版本太低一般都会有问题。比如使用jQuery1.8等等，如果这个组件版本太低，你可以认为这个代码应该很久没有更新没人维护了。
- 组件版本高低不是有问题的充分条件，因为低版本的组件可以backport一些安全漏洞也是安全的。如果要验证可使用工具versions、DependencyCheck、retire.js等来扫描。
- 最近有报告指出Docker Hub官方镜像76%存在CVE漏洞。
- 预防：使用自动化部署，固化大版本，小版本保持更新。定期再更新到大版本。

## OWASP A10:2017 –不足的日志记录和监控

- 不足的日志记录和监控，以及事件响应缺失或无效的集成，使攻击者能够进一步攻击系统、保持持续性或转向更多系统，以及篡改、提取或销毁数据。大多数缺陷研究显示，缺陷被检测出的时间超过200天，且通常不是通过内部流程或监控检测而是外部介入。
- Log可以多写，中心化Log，在Log中寻找征兆，做监控。并在出问题后溯源。
- 黑客的GET请求会被Web服务器Log，使用POST的数据一般不会被Log。
- 使用框架本身提供的Log服务。精确判断确定哪些进入数据库，哪些进入Log。Log都是很类似的，有些如果你需要用户在页面上可以查询到的应当放到数据库，而有些一辈子都可能用不到的就放到文件或NoSQL数据库里。

## CWE-352: 跨站点请求伪造（CSRF）

- 跨站请求攻击，简单地说，是攻击者通过一些技术手段欺骗用户的浏览器去访问一个自己曾经认证过的网站并执行一些操作（如发邮件，发消息，甚至财产操作如转账和购买商品）。由于浏览器曾经认证过，所以被访问的网站会认为是真正的用户操作而去执行。这利用了Web中用户身份验证的一个漏洞：**简单的身份验证只能保证请求发自某个用户的浏览器，却不能保证请求本身是用户自愿发出的**。
- CSRF有个例子是你有一个投票网站，恶意的人可以在他控制的一个大的访问量网站内构造提交到你投票网站的Form，所有访问了这个大的访问量网站的人都会默默投票。或者可以构造一些转账，关注粉丝，收藏宝贝等提交操作。
- 是否有CSRF token，框架一般都会提供，CSRF token是服务端对称加密生成的，通过浏览器的同源策略保障不被其他网站恶意读取。

## CWE-400: 不受控制的资源消耗（“资源枯竭”、“App拒绝服务”）

- 类DDOS，网络流量的DDOS你是没法控制的，你要预防的是一些你自己程序造成的DDOS。比如某风曾经由于某个域名无法解析客户端重试太多次导致国内互联网瘫痪。

## CWE-434: 未限制上传文件类型

- 严格限制上传文件类型，重命名文件名，获取扩展名方法应当正确。

## CWE-451: 用户界面（UI）对关键信息的误传（点击劫持和其他）

- 点击劫持，假设你访问一个恶意网站，这个恶意网站把某宝用IFrame嵌到页面上，设置为透明。并且把某宝收藏宝贝的按钮对准在某篇诱惑力较大的新闻或者下载的链接上。然后你点击恶意网站，实际上也点击了某宝的收藏宝贝的链接。预防，写X-Frame-Options。
- 某信，在给别人转账时提示信息后台使用类似，“向%s转账”.format(‘用户名’)，恶意用户可以用户名起为“着夕阳奔跑给您”，这样子用户就会看到类似“向着夕阳奔跑给您转账”的字眼，干扰用户的判断。
- 某音，假设有一天你发了个视频，然后去睡觉了，醒过来收到一条信息，“郑海山等110万人关注了你”，你以为一觉醒来就变成网红了，但是当你去打开那个信息，你发现只有一个人关注你，那个人名字叫“郑海山等110万人”。
- 我们传递给用户的信息一定要精确区分哪个是用户可能可以控制的，必须跟我们自己的文本分开，比如用颜色区隔，或者字体大小。

## CWE-601: 未经验证的转发和重定向

- 一些OAuth类的服务器会把一些code不小心传给其他网站，在自己服务器要做跳转时需要确认，可以使用白名单机制。比如某乎，跳转其他网站均会提示跳出。某宝和某信，商品详情页和公众号无法跳转。

## CWE-799: 交互频率控制不当（反自动化）

- 防止自动化投票，选课，利用一些其他现实的规则或逻辑完成程序上控制不了的能力。比如某窝，恶意爬取别人网站的评论等被人下毒。
- IP限制正确处理X-Forwarded-For和IPv6。X-Forwarded-For是上游反向代理给你传递的，反向代理必须加白名单。IPv6地址数据巨大。

## CWE-829: 不可信控制域包含功能性（第三方内容）

- 第三方内容在本地固化。

## CWE-918: 服务器端请求伪造（SSRF）

- SSRF，Server-Side Request Forgery，服务端请求伪造，是一种由攻击者构造形成由服务器端发起请求的一个漏洞。一般情况下，SSRF 攻击的目标是从外网无法访问的内部系统。漏洞形成的原因大多是因为服务端提供了从其他服务器应用获取数据的功能且没有对目标地址作过滤和限制。
- 是否提供向别的URL获取信息的功能，导致内网信息泄露。比如某搜索引擎，提供图片搜索功能，允许用户上传一张图片或者一个图片URL，通过恶意构造内网URL，可以获取到内网的一些信息。

## 其他

- 扣款金额是否可以为负数？能否抗击重放攻击？
- 比如我写的一篇《一个靠谱的微信公众号应用应当有什么特征》，提到了，如果你微信绑定了一个账户，你应当给用户显示你微信绑定过哪些账户和你目前绑定的账户被哪些微信号绑定过。
- 比如互联网有个大公司密码泄露，有些公司看到了就会松一口气，说，幸好不是他们。但是靠谱的公司就会花钱把泄露的账户包买下来，比对自己的账户，做撞库测试，如果发现你的账户密码跟泄露的一致，就会启动锁定或者密码取回机制。这是为用户考虑的安全，虽然这些不是你的原因，但是你可以为用户做得更多。

有错误告诉我，微信公众号没法更新，今后放到别的渠道吧。

如果你都能看到这里了，年轻人，我看你骨骼精奇，是万中无一的码学奇才，维护世界和平就靠你了，我这有本秘籍《如来神掌》，见与你有缘，就。。。

![](/images/2018/gongfu.jpg)