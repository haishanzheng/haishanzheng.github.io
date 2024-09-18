---
title:  "手机最重要的4个密码"
date:   2024-09-18 08:12:33 +0800
---

以下是手机最重要的4个密码：

- ① 开机、解锁和关机密码
- ② SIM卡密码（PIN码）
- ③ SIM卡PUK码，这个我不认为是密码
- ④ SIM卡服务密码

接下来我会介绍这几个密码是什么，为什么重要，有哪些被攻击的点和如何预防。

## ① 为什么手机一定要有开机密码（指纹、人脸等）

手机已经是个人数据中心了。如果按十几年前比喻，你拿着手机就类似你背着台式机、MP3、PDA、数码相机、银行卡、钥匙串、现金、老婆的照片在身上。

![](/images/2024/mobile/tsj.png)

所以一定要像电脑一样，设置BIOS密码、开机密码和全盘加密密码。当然，很多手机本来就是全盘加密的，如果你自己再插入存储卡，建议也启用全盘加密。

不建议将已经开机的手机交给陌生人操作，即使眼睛盯着也不行。有一种可能是对方可以往手机Type-C口插入一个无线设备，就有可能在你手机上安装一些应用。如果必须要将手机借给陌生人打电话，可以进入“维修模式”。

开机密码有数字、图案、蓝牙、指纹和人脸等，其中蓝牙、指纹和人脸无法抵抗物理逼迫，可以结合着使用。

关机密码跟开机密码是一样的，只不过你要设置关机时验证。加个验证，不会增加多少麻烦，因为只有锁屏时关机他才会验证密码。他可以防止你手机丢失后有人要恶意关机。或者在你手机做什么操作引起界面异常后通过重启来重置状态以掩盖痕迹。

当然关机密码无法阻止手机被丢进一个法拉第笼包包里。

## ② SIM卡密码

SIM卡里面有IMSI（国际移动用户识别号，International Mobile Subscriber Identification Number）、ICCID（集成电路卡识别码，Integrate circuit card identity）、Ki、Kc等密码计算需要用到的数据，他唯一没有的就是电话号码。SIM卡被插入手机后，会把IMSI、IMEI（国际移动设备识别码，International Mobile Equipment Identity）、Kc等给远程做类似非对称加密的验证，一旦验证成功后，运营商根据IMSI号在数据库里查找绑定的手机号码，你就算入网了。

SIM密码是为了保护IMSI、Kc等等数据的。

![](/images/2024/mobile/sim.png)

SIM卡密码可以防止在你手机离开你一段时间内有人可以通过拔插SIM卡来放到别的手机上，通过收手机验证码登录某些保护性较低的App；或者用来攻击短信为双因素认证、取回密码的账号；或者用来打电话给你的朋友进行诈骗活动；或者查询你的通话记录或进行各类设置的（比如几年前可以将短信都转发到某个邮箱）；或者发微博类似的（几年前）。

我们手机里面有几类App，一种是类似微信、淘宝的，你经常要打开，开过一次后，就不需要密码了，一种是银行类App，你偶尔打开，所以打开还要验证。一种是中等程度打开的。

一般安全性要求高的App，也会验证原先写到手机里的私钥。比如很多银行的App，或者微信、支付宝，换了手机，初次登录无法单纯使用短信验证码登录，会起双因素挑战。

### SIM卡密码=PIN码

SIM卡密码一般只有4位。他是PIN码，叫PIN码的，一般不会在互联网传输。

SIM卡就是非常典型的无密码设计方案。

### 无密码方案

密码有2个特性，密码输入的次数越多越不安全；密码输入的次数太少也不安全。因为你会忘记这个密码。所以没有密码是最好的。

无密码也不是说没有密码，只不过密码变成一串随机、无比长的字符串，不需要你输入，但是需要你通过某种简便的方式来授权输出，比如PIN码，比如受信任的客户端等等。

一般我们要登录一个网站，输入一个密码后，网站就登录成功了。无密码方案类似，也是输入一个密码后，网站就登录成功了。

虽然看起来一样，但是底层实现机制是不同的。都是输入密码，但是前面一个输入的密码要在互联网上传输到服务端去验证，而无密码方案的，输入的密码只是激活本地的密钥管理器，密钥管理器不会发送自己的密钥到服务器，只会要服务器给一个挑战字符串，密钥管理器加密后传输到服务器，服务器解密看能否恢复原先给的挑战字符串，所以没有密钥在互联网上传输。

更技术一点说明，这是一个经典的非对称密码的应用，只不过App帮我们处理好密钥的分发、存储和使用。

## ③ PUK码

PUK码是在你SIM卡密码输入错误达到一定次数后用来解锁SIM卡的。

这是因为SIM卡自身带有防止暴力破解PIN码。当然，SIM卡也会防止暴力破解PUK码，一旦PUK码输入超过多少次，他就会自毁。这个不是爆炸自毁，只不过把自己清空而已。

![](/images/2024/mobile/mi.png)

这时候你就必须去营业厅换卡。

### 这些保护都很弱

虽然你看到好多密码在保护SIM卡，但是有些保护不会非常强。

比如PUK码，你可以从以下几个渠道获得你的PUK码。

- 买SIM卡时大卡会印刷PUK码，可能会遮盖
- 某运营商，打电话后会客服会直接报给你本机的PUK码
- 非本机，用绑定了同一个身份证号码的手机打电话，也可以给你PUK码
- 输入目标手机的服务密码后，可以给你PUK码
- 带身份证去营业厅，可以给你PUK码

而且这个PUK码通常不可变更，除非换SIM卡。

SIM卡还有被复制的风险，随着技术的进步，SIM现在已经很难被复制了，或者只是说，我所知道的。即使可以，那应该是非常高级的攻击，你不会遇到，如果你是非常高级的人，你会有其他措施来保证这个。

越高级的制式、越新的SIM卡，抗攻击的能力越强，所以有5G就不要用4G，千万不要用2G，定期去营业厅换个SIM卡，来重置PUK码和升级SIM卡自身。

为什么不推荐用2G，在2G时代，SIM卡是单向校验的，也就是运营商有能力来检验你的SIM卡对应到某个手机号码，但是你没有能力检验这个运营商是不是合法的运营商。所以在2G时代，我们会遇到“伪基站”的攻击。进入3G、4G、5G后，都是双向校验（类似mTLS），SIM卡先确认这个是真的运营商后，才进行入网检验。当然，有时候你还会遇到降级攻击，也就是，通过屏蔽信号，让你进入2G。所以2G网络是必须关闭的，运营商已经慢慢开始关闭2G网络。

几年前还有人拿着假身份证在异地把SIM卡办出来（SIM swap attacks）。现在三大运营商越来越规范了，但是一些小的，虚拟的运营商，可能防护就不会这么好。

## ④ SIM卡服务密码

SIM卡服务密码，一般是用来你办理各类业务需要使用到的密码。这些渠道可以是电话、自助机或者Web网站，需要验证你的服务密码。

SIM卡服务密码和短信验证码可以构成一个双因素认证。但是，我所知道的某个运营商，可以通过手机编辑短信，输入身份证即可重置服务密码，这是不安全的。

SIM卡服务密码也有防止暴力破解的机制，某运营商，一天输入几次后会被锁定，零点才重置错误次数。

## 如果你决定设置SIM卡密码后，注意应急预案

如果你看到这里，迫不及待地去设置SIM卡密码，有几个前提你一定要知道。否则你就像我某次一样很尴尬，才催生了这篇文章出来。

以下是应当注意的点：

- 在设置SIM卡密码之前必须获得SIM卡PUK码和服务密码并记录下来
- 必须验证服务密码的有效性
- 必须验证PUK码的有效性，验证很容易，故意输错多次SIM卡密码，看能否通过PUK码解锁
- 开机密码6-8位。SIM卡密码4位，PUK码8位。在你开机的时候，一定要辨认好界面，让你输入的是哪个密码，通常需要辨认的是开机密码和SIM卡密码
- 设置后多次重启，演练一下输入的过程
- 有些手机会将SIM卡密码记录在手机里，帮你输入
- 尽量在工作日，不是出差，带着身份证，离营业厅近的位置设置和变更这些密码