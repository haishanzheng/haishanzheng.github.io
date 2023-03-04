---
title:  "谈 X-Forwarded-For 的IP伪造问题"
date:   2021-07-02 16:55:01 +0800
---

## 谁需要阅读本篇文章，知道这个风险

- 反代管理员。
- Web服务器运维人员，XFF可以在Web服务器直接处理掉，或者传递给开发自行处理，你需要跟开发协商好。有些业务源代码无法修复，只能在Web服务器层处理掉。
- 业务系统运维人员，如果系统不是你开发的，系统内又有需要IP地址做鉴权和审计，你需要去验证产品是否有规避这个问题。
- 开发，如果需要IP地址做鉴权和审计。注意平衡好可扩展性和安全，简单点，跟反代管理员、Web服务器运维选定一个字段后固化，不需要考虑所有字段。
- 安全设备运维，你在安全设备上看到的来源IP都是反代，你的设备或者无法解开XFF，或者过分信任XFF。

## X-Forwarded-For是什么

- 2017年，在 [关于投票、点赞、支付、微信，安全性](https://dog.xmu.edu.cn/2017/12/19/crwal.html)，我举了某个运营商的某个投票系统存在地址伪造问题。
- 2018年，我在 [继续说IPv6、HTTPS、HTTP/2，系列完结吧](https://dog.xmu.edu.cn/2018/06/19/deploy-nginx-with-ansible-done.html) 里面提到了真实地址获取应当注意的内容。
- 2020年，在 [自动化部署分布式课程管理平台Moodle](https://dog.xmu.edu.cn/2020/03/11/moodle-server-clustering-automatic-deployments.html) 的 “客户端IP获取安全配置”里，我详细介绍了一个PHP程序在部署在反代后面获取IP地址正确的姿势。

但是，我发现，还是有很多人不知道这个东西，或者知道有这个东西，但是了解不够深入，没有真正去规避好这个IP伪造问题。

这个问题的根源在于：**不要信任用户的任何输入**。

我一直强调一个带认证功能的应用交付设备的重要性，包括CDN等越来越流行，关于真实地址获取这个话题，**也越来越需要引起重视了**。

因为有了众多网关类设备的存在，用户跟服务器之间逻辑上物理上其实都已经不是真正意义的点对点通信了，所以XFF承担了在网关类设备之间传递用户真实地址的作用。

网关类的应用交付设备或者代理跟远端通信，起手都会说：“我有一个朋友。。。”，然后他可能会告诉你这个“无中生友”的姓名，这个告诉的过程，和这句话本身一样不可信。

![](/images/2021/cst.jpg)

技术一点说，就是XFF是在HTTP header里面的一个非标准化以X-开头的字段（真正的规范是Forwarded，rfc7239），一旦经过一个代理（正向、反向），代理就会把跟他连接的设备IP地址使用“逗号+空格”加到XFF字段内，继续往后传递。也就是，最后到开发手里，可能会是这么一个字符串：

```sh
X-Forwarded-For: client_ip, forward_proxy1, forward_proxy2, reverse_proxy1, reverse_proxy2
```

## XFF带来的安全问题

**代理导致原始信息丢失，有网关类的都会存在这个问题**，这个信息有些是丢失了，有些是功能，就是为了隐藏。

不止HTTP，邮件系统也使用了这个字段。DNS也存在这个问题，我们知道我们要访问 dog.xmu.edu.cn ，电脑向DNS递归服务器请求地址，由DNS递归服务器向外一级级获取。所以对于权威DNS来说，他只知道DNS递归服务器的IP地址，他也不知道真实客户端的IP地址，这会导致根据来源返回的IP跟真实情况不符合。所以DNS新近引入edns-client-subnet扩展来解决这个问题，当然，他也会带来欺骗导致权威服务器内部DNS信息泄露问题，这里不展开。

甚至很早以前“来电显示任意号码修改”，其实也都是类似问题引起的。

怎么判断有没有处理好XFF问题，最简单就是，在配置里面找是否可以设置信任IP白名单的配置，如果没有，基本上就是废了。

- XFF信任链没有建立，导致XFF在header字段内被任意伪造
- XFF地址不是操作系统类似$_SERVER['REMOTE_ADDR']给你的，所以你必须处理可能的注入和XSS等问题。

## 我做了一个试验

为了完成这篇文章，我设计了一个庞大的可重复的试验，我动用了9台虚拟机，写了一些自动化部署脚本，一些测试用例，公开了源代码，输出了一堆表格：

- 一台可以跑curl伪造header的Ubuntu客户端和ModHeader插件
- 2台虚拟机，安装正向代理Squid和NGINX
- 安装了2台反向代理，分别尝试并行和串行
- 安装了4台服务器，分别安装NGINX、Apache、IIS、Tomcat
- 我通过开关切换ngx_http_realip_module和Apache的mod_realip和Client IP Header和RemoteIpValve
- 我观察上面Web服务器的日志记录
- 在上面4台服务器跑在FastCGI和php-fpm模式下的PHP
- 和WSGI的Python
- 和Java
- 和启用了django-ipware模块的Python
- 观察phpinfo()，和Python的多个获取的字段：
  - HTTP_X_FORWARDED_FOR
  - X_FORWARDED_FOR
  - HTTP_CLIENT_IP
  - HTTP_X_REAL_IP
  - HTTP_X_FORWARDED
  - HTTP_X_CLUSTER_CLIENT_IP
  - HTTP_FORWARDED_FOR
  - HTTP_FORWARDED
  - HTTP_VIA
  - REMOTE_ADDR
- 我试验了多个字段同时存在的优先级
- 我试验了各个地方尝试注入

以上纯属虚构，我其实没做。。。

## 最佳实践

太复杂了，我们以一个典型的高校可能的架构来分析。

- 一台应用交付，99%只需要简单将XFF重置成客户端IP往后传。1%需要应对部分应用还加了互联网CDN，这里必须信任CDN地址并且解析了CDN前面的客户端IP地址往后传。如果不想处理，直接应用不上应用交付。
- 没有串联第二台应用交付。如果有，需要proxy_add_x_forwarded_for

一般的高校，升级到反代的架构后，我们首先明确反代管理员不会傻到用proxy_add_x_forwarded_for。接着各个应用还是最原始的使用REMOTE_ADDR，获取真实IP不正确，这个风险不高，只是反代IP一般是在校内，可能导致本来只向校内开放的资源的限制失效。也还好。

如果有应用开始想要使用XFF了，需要注意以下的安全问题。

### 反代管理员

99%无脑重置XFF，为什么要重置？你需要帮后端挡住伪造。你可能认为client_ip, forward_proxy1, forward_proxy2好像有一点价值存在，但是实际上意义不大，正向代理一般不会提交内部的IP地址。所以简单点，直接客户端设备IP往后传。

如果为了防止后端组件或者开发误使用了其他包括HTTP_FORWARDED、HTTP_VIA、HTTP_X_REAL_IP等字段，可以在这里也直接重置这些字段。

### 服务器管理员

- 配置防火墙只允许反代访问HTTP端口，将自己在互联网隐藏起来。
- 如果反代的IP不固定，比如互联网CDN，IP多，更新频繁。（我们在用的一个CDN也很不行，每次更新增加新的IP段，只会给某个邮箱发送信息。最近多了一个，会把所有IP作为附件发送，但是编码是GB2312。。。从这里，基本上你可以知道提供者和使用者的水平了）。然而即使你有CDN的地址，你也不能防火墙设置只允许CDN IP访问，因为有时候会有时间差，CDN IP先上线，再通知你。而且如果一旦你的流量超过CDN的限额，续费之前，可能CDN直接将你降级，把你的IP直接发布在DNS上，你需要直接对外提供HTTP服务。
- 需要将反代IP加入本机安全设备白名单，以免被误判阻断。
- 为了日志记录合规，虽然你也可以从反代查到日志，但是将真实IP多记录一份到日志，害处不大，还可以从模块级帮开发解决XFF问题。
- 配置模块时一定要正确配置信任IP为反代的IP。
- 定期测试反代管理员配置有效性。

### 开发

- 前提是，宁愿没获取到，不能被伪造。
- 如果可以，不要使用基于IP的限制。
- 最完美的情况是对于开发来说，应用交付和Web服务器都配置完善，无脑使用REMOTE_ADDR。但是你也要预防前面任何一个配置有可能配置错误或者重新安装后导致的失效问题。
- 校内反代的IP一般也是校园网IP，所以如果没有正确处理，可能导致面向校内IP资源的内容被错误全部对外发布。
- **为了对反代友好，同时保留自己独立提供HTTP服务的能力，所以代码里面必须从信任IP连接内获取XFF字段，如果没有，就直接记录客户端设备IP。**

```python
# 这里只考虑只有一层反代的情况，并且这个反代重置了XFF
def get_client_ip(request):
    client_ip = request.META.get('REMOTE_ADDR') 

    if client_ip in TRUSTED_PROXY_IP_POOL:
        if request.META.__contains__('HTTP_X_FORWARD_FOR'):
            x_forwarded_for = request.META.get('HTTP_X_FORWARD_FOR')
            client_ip = x_forwarded_for.split(',')[0].strip()

    return client_ip
```

- 如果反代信任IP无法获得，但是可以明确自己一定在n个反代后面，则可以简单丢弃n - 1个数的XFF IP列表即可。
- 常用的组件级的有些都没有正确处理好IP欺骗，比如django-ipware。最好认真阅读源代码。
- 数据库IP地址字段要足够长。可能需要处理字段内不止一个IP地址的情况，可以分2个字段来存储。
