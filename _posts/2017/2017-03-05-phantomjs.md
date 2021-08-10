---
title:  "使用PhantomJS对网站截图和遇到的一些问题"
date:   2017-03-05 19:50:12 +0800
---

最近写备案系统，备案系统其实就是将厦门大学所有网站都做个备案，以备在网站遇到安全问题可以联系到人处理。由于有了备案，有了网站列表，顺便就可以生成网址导航，跟现在的厦门大学校内网址导航 http://123.xmu.edu.cn 一样。目前旧版123网址导航（数据有点老，新的还在开发中）我是通过VS.NET，写了个Form，在上面放了个WebBrowser，对数据库的每个网址，均去Navigator一下，获得ScrollRectangle.Width和ScrollRectangle.Height，等待一段时间，然后截图，可以截取下首页的整个页面 http://123.xmu.edu.cn/all.php 。

备案系统是在Linux下用Python写的，所以我还需要新建一台Windows虚拟机，在上面计划任务跑个程序，还需要去Linux服务器获取网站列表。算是很麻烦，但是可以解决。

后来在跟姜开达聊到备案系统，他说“海量网站截图我已经在做了，定期快照取代码和截图。”我就提到截图有技术难度。然而

![](/images/2017/phantomjs/250pxbuxiangshuohua.jpg)

于是我就开始研究了。

> PhantomJS is a headless WebKit scriptable with a JavaScript API. It has fast and native support for various web standards: DOM handling, CSS selector, JSON, Canvas, and SVG.
> 
> Programmatically capture web contents, including SVG and Canvas. Create web site screenshots with thumbnail preview. 

很快写了段代码，发现就解决了。解决了。

	"use strict";

	var page = require('webpage').create();
	var system = require('system');

	var address = system.args[1];
	var output = system.args[2];

	var pageWidth = parseInt(system.args[3], 10);
	var pageHeight = parseInt(pageWidth * 3/4, 10);
	page.viewportSize = { width: pageWidth, height: pageHeight };

	page.open(address, function (status) {
	    if (status !== 'success') {
	        console.log('Unable to load the address!');
	        phantom.exit(1);
	    } else {
	        window.setTimeout(function () {
	            page.render(output);
	            phantom.exit();
	        }, 200);
	    }
	});

简单得令人发指。下面是PhantomJS的截图效果，非常赞。

厦门大学主页PC端：
![](/images/2017/phantomjs/xmu.jpg)

厦门大学主页手机端：
![](/images/2017/phantomjs/xmum.jpg)

幸好程序进度拖得久了，幸好还没开始写（是的，现在都还没开始写），幸好跟姜开达交流了下。如果不是，我是否会继续使用老技术，或者会Google下“网页截图 linux？”关键字呢？我是2013年写的123，查了

> PhantomJS 1.5 “Ghost Flower” was released on March 20, 2012. It added pure headless (no X11) for the Linux version, improved troubleshooting with interactive mode and remote debugger, and a new system module.

为什么这么多年这个技术对我都在盲区？如果我做自动化测试，我可能会去考虑用Selenium，现在又多个选择了。

说完了，现在开始说但是了。任何事情都不是一帆风顺的，配置过程中也遇到了一些坑，分享出来。

# 包管理器安装
为了达到自动化部署，我一般都喜欢用包管理器安装组件，然而在我安装完Ubuntu 16.04LTS最新的PhantomJS包后，无法启动。提示错误：

	QXcbConnection: Could not connect to display 

查了下，必须加个环境参数。

	export QT_QPA_PLATFORM=offscreen

而官网下载的就没有这个问题。不知道为啥Ubuntu维护的包会有这个问题。

# 中文支持
Ubuntu自带的PhantomJS，即使再安装

	sudo apt-get install xfonts-*

也是无法认到中文的，原因是里面打包的Qt不知道为什么即使设置 QT_QWS_FONTDIR 也无法变更。所以必须

	ln -s /usr/share/fonts/某个中文字体目录 /usr/lib/x86_64-linux-gnu/fonts

而官网下载的只要按以上安装xfonts-*就可以完美支持中文。

> http://doc.qt.io/qt-5/qt-embedded-fonts.html
>
> Qt normally uses fontconfig to provide access to system fonts. If fontconfig is not available, e.g. in dedicated embedded systems where space is at a premium, Qt will fall back to using QBasicFontDatabase. In this case, Qt applications will look for fonts in Qt's lib/fonts/ directory. Qt will automatically detect prerendered fonts and TrueType fonts.

# examples/rasterize.js
包管理器下载的和官网下载的示例文件均有错误。GitHub上的已经修复。直接运行会提示：

	ReferenceError: Strict mode forbids implicit creation of global property 'pageWidth'

这个问题是由于某次修改加入

	"use strict";

导致的，但是"use strict";理论上不是应该第一次就加入么。。。
修复很简单，
第4行加入    pageWidth, pageHeight; 定义即可

# WebKit的console.log问题
很奇怪的是，不知道PhantomJS打包的是什么版本的WebKit，在我调试时发现一个很奇怪的现象，至今无法解决，由于不影响使用，也懒得去研究看源代码了。

这个问题是，理论上标准的console.log是支持格式化替换输出的，以下是标准的console.log的语法：

> console.log(obj1 [, obj2, ..., objN]);
>
> console.log(msg [, subst1, ..., substN]);
>
> msg
>
> A JavaScript string containing zero or more substitution strings. 

也就是，假设你有一句

	console.log('i am %s.', 'dog');

在Chrome、Firefox最新版都会输出

	i am dog

而包管理器下载的PhantomJS输出是

	i am %s

官网下载的输出是

	i am %s dog

也就是包管理器只看到了第一个obj1，后面的参数全部被忽略，官网下载的看到了所有的可变参数，但是不支持substitution替换。
如果你知道为什么，请告诉我。



