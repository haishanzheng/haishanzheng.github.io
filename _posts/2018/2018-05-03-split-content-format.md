---
title:  "内容和格式分离，说说写论文、写文档、画框图的事"
date:   2018-05-03 15:28:09 +0800
---

内容和格式一定要分离，**计算机的发展史就是不停拆封不停分层，达到各层松散耦合，各自独立高速发展的历史**。前段时间在热炒我们没有自己的芯片，没有操作系统，我倒觉得没什么，操作系统脏活累活人家都干了，你OTT了别人应该偷笑吧。而且人家是Open Source的，自己做一个会Open么？不见得。其他该担心的事情还多着呢。只是一个Word软件导致某城市采购国产操作系统国产文档编辑工具政务无法开展下去我就有点不明白了，国产软件真的不好么？兼容性真的有问题么？不能妥协么？培训不到位么？应该是其他非技术性的原因吧。

Word是最邪恶的发明，他的所见即所得降低了写漂亮文档的门槛，任何一个人都可以花很多时间写出格式非常漂亮的文档，然而很多人也就止步于此，表面上看起来光鲜亮丽的门面，隐蔽工程如何呢？而且很多人实际上没用到Word多少的功能，市面上很多Word的书厚厚一大本，很多有点基础的人以为Word这么简单的东西我还需要学？太年轻了。

接下来我介绍一下我自己内容和格式分离的例子，这些工具实际上很多，每种都有很多替代品，我只提我自己使用的。

为什么要做到内容和格式分离？其实对很多人也无所谓。对于我来说，我可以完全掌控所有的代码，掌控最后生成的格式，我可以对代码进行版本化，可以专心于内容创作而不用关心格式。

![](/images/2018/split-of-content-format/westworld.jpg)

## HTML5+CSS

内容和格式分离我们最常见的就是HTML+CSS了。

## 论文，Word

- Word一定要用到样式，定义新的多级列表，将级别链接到样式。
- 最好还能打开导航窗格，我一般看大文档写得是否专业就看这个。
- 编号必须自动生成。
- 如果写论文，必须使用交叉引用。可以用NoteFirst来自动维护引用。NoteFirst可以自动更新题录，下载全文，写中文引用还是蛮方便的。
- 一旦你使用了样式，你就可以很方便针对不同期刊调整格式，一家被拒可以很快换另外一家。

比如说我的《基于开源软件的DNS查询日志分析系统》这篇论文，你在CNKI下载到的可能是个打印效果的PDF，但是我的原稿是这样子的：

![](/images/2018/split-of-content-format/dns-style.jpg)
![](/images/2018/split-of-content-format/dns-toc.jpg)
![](/images/2018/split-of-content-format/notefirst.jpg)

## 论文，LaTex

如果能用LaTex来写论文更好，比如IEEE就有特殊的双栏模板，https://www.ieee.org/conferences/publishing/templates.html 下载下来，安装一下TeX Live，5分钟就可以输出非常完美的PDF。

如果你都用上LaTex了，我也没什么可以跟你说的了。TeX Live的BibTeX稍微配置麻烦了一点点，不过懂了也是很快，配合Google Scholar，使用BibTeX或者EndNote管理引用。

## 写文档的格式

Git的盛行，使得Markdown越来越流行了。当然还有Rich Text Format（RTF），reStructuredText等等非常多。建议首选Markdown。

## 写博客

博客为了达到内容和格式分离，同时内容可以进Git，可以选择轻量级的使用Markdown格式编写内容，输出成静态HTML的工具，比如Jekyll，Hugo等等。微信公众号，好像还不支持这些格式？

## 写文档

GitBook，MkDocs，Sphinx，Doxygen，readthedocs.org。这些或者使用各种不同的格式编写，或者可以直接从源代码抓出注释形成文档。

## 源代码的Highlight

源代码通过后期highlight.js格式化，也是一个内容和格式分离的例子。

## 表情 Emoji

类似这种的 :smile: ，最后会格式化输出一张图片。

![](/images/2018/split-of-content-format/smile.png)

这里有张小抄，列出了大部分网站支持的表情图标， https://www.webpagefx.com/tools/emoji-cheat-sheet/ 。

## PPT

PPT要做到内容和格式分离挺难的，因为PPT做得好的话是没有文本内容的。。。不过也有一些H5的可以做到完全从头开始写代码，并支持浏览器演示的，比如nodePPT、Reveal.js、impress.js等等，适合内容比较多的技术文档分享。

## 流程图、框图

mermaid是使用文本写流程图的格式。在线的在 https://mermaidjs.github.io/mermaid-live-editor/ 。你可以写个类似

    graph TD
        H[郑海山] --> |碎片时间| W(找点事情做吧)
        W --> T{让我想一下}
        T --> |1| D[刷抖音]
        T --> |2| E[刷公众号]
        T --> |3| F[看淘宝头条]

生成这样子的图。

![](/images/2018/split-of-content-format/mermaid.jpg)

类似的在线还有 http://asciiflow.com/ 、 https://textik.com 等等。

![](/images/2018/split-of-content-format/asciiflow.jpg)

## UML在线画

这种一般适合你如果只画一张图的时候，如果是项目还是建议使用专业的软件来管理多个UML图。

在线版 https://yuml.me/diagram/usecase/draw

你可以写：

    [人]-(Born),
    [人]-(Eat),
    [人]-(Drink),
    [人]-(Die),
    (Drink)>(Coke),
    (Drink)>(Traditional Chinese medicine),
    (Traditional Chinese medicine)<(Die)

会生成这样子的图：

![](/images/2018/split-of-content-format/uml.jpg)

## infographics

信息图就不要用Photoshop来画了，可以用 https://www.visme.co/make-infographics/ 这个来，有非常多的模板，选择一个，放上数据，就可以做出类似以下的效果：

![](/images/2018/split-of-content-format/infographics.jpg)

## 录屏软件

Record and share your terminal sessions。一般我们的录屏软件会生成视频，文件很大，观看的人要拷贝一些命令很不方便，所以你可以使用 asciinema 来制作录屏，生成的是文本格式，使用类似视频的播放模式，可拖动。

![](/images/2018/split-of-content-format/asciinema.jpg)

## 其他

其他类似Photoshop原图、苹果的.AAE文件，都是内容和格式分离的例子。

有错误告诉我，还有其他的也介绍给我，看完点个 :thumbsup: 吧。
