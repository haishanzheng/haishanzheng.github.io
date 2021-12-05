---
title:  "网络空间测绘：IP洞察-浏览器插件，Tampermonkey油猴"
date:   2021-12-05 15:40:01 +0800
---

以前我写了 [网络空间测绘：IP洞察](https://dog.xmu.edu.cn/2021/04/13/ipinsight.html) ，在里面，我提出了一个问题，**“在平时我们做应急处置时，看到一个自己IP，所有人对着这个IP很懵圈，他是谁，他从哪里来？他是干什么的？”**。

我找到了一个解决方法并最终实现了一个内网资产数据库，每天定期从各个系统去抓取IP的所有数据，提供给数据中心管理员或者说安全人员使用的工具，他可以看到跟这个IP和IP关联背后服务器的相关信息。

我用了一个被翻译为“洞察”的英文单词insight，并将项目命名为IP Insight。

在如何使用IP Insight时，我说道：

> 时间到了8点，上班的人们陆陆续续来了，郑工在态势感知平台里看到了一个IP，皱了皱眉头，在Controls里输入，Kibana赶紧把准备的数据关联在一起，五颜六色的Bar、Area、Line、DataTable、Metric、Pie在Dashboard跳出来。一切开始热闹起来了。

然后华侨大学顾鸿跟我说，你可以做个浏览器插件，IP都不用输入，移动上去，直接把IP相关内容展示出来。我豁然开朗，还能这样。。。于是我做了出来。

## 浏览器插件，Tampermonkey油猴

开始我试着用浏览器插件来做，但是我发现浏览器插件调试，发布都很麻烦，每次修改都必须重新reload一下插件。然后就算把插件写出来了，发布还得去微软或者Google注册，否则只能使用开发者模式，安全性不高。

然后我发现了Tampermonkey，油猴。

> The world's most popular userscript manager，Tampermonkey（油猴）是最受欢迎的浏览器扩展之一，拥有超过1000万用户。
> Tampermonkey用于在网站上运行所谓的用户脚本（有时也称为Greasemonkey脚本）。用户脚本是小型计算机程序，可以更改页面的布局，添加或删除新功能和内容或自动执行操作。

Tampermonkey跟浏览器插件的差别就是，他本身就是一个浏览器插件，你可以认为他就是一个插件管理所有插件。他简化了写浏览器插件的步骤。原先写浏览器插件，你需要遵守一些规则，并且要做好打包和分发的步骤，在Tampermonkey里面，你只要新建一个脚本，在线编辑脚本内容，就可以实现同样的功能。举个例子：

## 对漏扫设备的WebUI进行修改，加入内网IP地址显示

我们有一台漏扫，漏扫设备大家都用过，新建一个扫描任务，输入一些IP地址，扫描完等，点击任务来看看结果。这是列表：

![](/images/2021/vulscan1.png)

从这个界面，对IP不敏感的，看起来是没有任何意义的，你不知道这些IP后面哪些是价值高的资产，你只能看着大量的评分为10的IP着急。

当然，我还用了别的手段，我会下载XML格式的报表，然后用Python解析，通过备案系统，关联到联系人做分发。

我们也没有买这个漏扫设备的API模块，但是因为他的登陆没有双因素认证，也没有奇怪的验证码，所以我也可以控制一个Headless Chrome自动化登陆，填写IP信息，启动扫描，下载XML。让这一切更自动化。

对于漏扫，我也写过：

> [9]郑海山,屈斌,许卓斌.让二级学院也拥有漏洞扫描能力[J].中国教育网络,2018,(04):61-52. 摘要：通过安全服务外包和采购漏洞扫描设备并开放给二级学院管理员使用的方法，探索全民参与网络安全的道路。

这里不说这些，我写了个Userscript，找到结果IP地址，然后向远程IP Insight获取在内网资产内的所有字段，就直接显示在IP后面。脚本类似：

```javascript
// ==UserScript==
// @name         New Userscript
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       You
// @match        https://172.XXX/*
// @icon         https://www.google.com/s2/favicons?domain=tampermonkey.net
// @grant        GM_xmlhttpRequest
// @require      https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.6.0.min.js
// ==/UserScript==

(function() {
    'use strict';
    var $ = window.jQuery;

    $('.host_list td span a').each(function(index, obj) {
        const { groups: { ip } } = /\/ip\/(?<ip>.+)\//.exec(obj.href);

        GM_xmlhttpRequest({
            method: "GET",
            url: "https://172.XXX:5000/getByIP/" + ip,
            onload: function(response) {
                $(obj).append(response.responseText);
            }
        });
    });
})();

```

最终长这样：

![](/images/2021/vulscan2.png)

可以看到，红圈那些是我从IP Insight获取到的数据，一下子就可以关注到高价值服务器，提前通知，其他的，就让备案系统慢慢去分发了。

## Tampermonkey油猴的能力

HTML的巨大优势在于，代码都已经下载到本地了，你可以随意任意控制。所以，如果你看一个页面展示效果让你很不爽，你就可以写个Userscript来改他。

比如我们经常遇到过有些上报数据工作，由于上级部门的系统不一定有批量导入数据功能，很多人可能就一边骂一边手动一个个输入。

这时候，与其骂，不如直接写个Userscript，自动化导入数据。

我上面的2个截图IP没有后期模糊，但是都是假的。我直接写了下面的Userscript，把所有IP都替换成随机IP，加上随机IP Insight的数据，直接就截图出来了。

```javascript
// ==UserScript==
// @name         Fake VulScan IP Userscript
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       You
// @match        https://172.XXX/*
// @icon         https://www.google.com/s2/favicons?domain=tampermonkey.net
// @grant        GM_xmlhttpRequest
// @require      https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.6.0.min.js
// ==/UserScript==

(function() {
    'use strict';

    var $ = window.jQuery;
    var host_labels = new Array("OA", "主页", "邮件", "法学院托管1", "图书馆托管2", "郑海山测试", "郑海山个人主页");

    $('.host_list td span a').each(function(index, obj) {
        var ip = '' +
            (Math.floor(Math.random() * 255) + 1)+ "." +
            (Math.floor(Math.random() * 255))+ "." +
            (Math.floor(Math.random() * 255))+ "." +
            (Math.floor(Math.random() * 255));
        $(obj).html(ip);
        $(obj).append(
            ' <span style="float: right;">(' + 
            host_labels[Math.floor(Math.random() * host_labels.length)] + 
            ')</span>');
    });
})();

```

## TODO

- 一个通用功能，鼠标选择IP地址，自动出现关于IP的一切信息。
- 对特定页面进行完全自定义化，比如上面某漏洞扫描的例子。
- 为了性能，为了安全，@match必须写白名单。否则一个恶意网页，来个水坑攻击，可以让你把网内资产都暴露出去。
- 获取内网资产的API，必须限制严格的IP地址或者AccessKey保护。

从看到这篇文章开始，立刻安装一个Tampermonkey吧，如果你还没用过的话。
