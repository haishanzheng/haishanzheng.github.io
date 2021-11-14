---
title:  "所见非所得，Unicode带来的一些“眼见为虚”的安全问题"
date:   2021-11-14 23:23:12 +0800
---

## 太长不看

这篇文章我用了十几张图，主要谈到了以下几个“眼见为虚”的安全问题和2个可以让你简单“动手试一试”的例子：

- Windows文件名RLO导致你放心打开txt变成打开可执行文件bat、exe的问题
- Unicode不可见控制字符带来的双向排版问题
- 利用可见屏幕显示不下所有字符插入垃圾代码问题
- 同形文字攻击问题，比如@和＠。

## 脑筋急转弯

首先我们来看一个脑筋急转弯

![](/images/2021/trojan-source-attacks/ypxc.jpg)

是这样的

![](/images/2021/trojan-source-attacks/ypxc1.jpg)


等等

等等

等等

答案是，中文有些店招是从右往左读的。。。

## 为什么会有双向文稿排版控制字符

世界上比如阿拉伯文或希伯来文，或者我们古代的文字，是从右向左的。如果你完全在阿拉伯文环境下使用操作系统去编辑文本，那是没有问题的，但是如果你在从左向右的环境要去编辑一个最终是从右向左显示的文本，你可以选择直接在文本层面反转，或者按正常顺序写文本，然后加一个控制字符。

> 已知最早的商代甲骨文主要文字排列是由上自下，左右顺序没有明确的规定。据推测，纵向由右至左的排列方式主要来自于书写竹简时右手持笔，左手持简并顺势将其排列的方式

> 最早提议汉文横书的是厦门大学校主**陈嘉庚**先生。那是1950年6月在全国政协一届二次会议上，陈嘉庚提案建议中文书写统一由左而右横写。

ASCII 7位只能表示128个字符，类似我们中文以前只能用GB2312标准，用2个字节进行编码，而Unicode编码方案为字符集中的每一个字符指定了统一且唯一的二进制编码，解决了跨语言、跨平台的交流问题，就是有点像IPv4和IPv6的差别。因为Unicode编码可容纳字符实在太多，就随便浪费，就加入了一些诸如双向文本控制等可能很多人一辈子都用不到的控制字符。

## 所见非所得

> 施一公说，你用眼睛能看到的东西，能感受到的能量存在形式加在一起，只不过是我们这个宇宙空间4%的存在形式，也就是说96%你既看不到也感觉不到，但都是是客观存在。蜜蜂很了不起，可以看到紫外，你以为蜜蜂采密看到是鲜花，它看不到鲜花，而是看到的紫外线。

然而**即使你眼睛看到的，都不一定准确。**

这让我想起20年前我在上Pascal算法课的时候，那时候都是DOS，没有图形界面的，老师在课上布置一道题目，要求大家现场写出来，然后写完的举手让老师过来运行看看结果正确就可以回宿舍了。正常的一个经典八皇后DFS回溯算法是这样子的（以Python为例）：

![](/images/2021/trojan-source-attacks/nqueens-correct.png)

，输出类似这样子：

Q.......

....Q...

.......Q

.....Q..

..Q.....

......Q.

.Q......

...Q....


但是有一个同学（别问我是怎么知道的），他的解法类似这样的，然后提前走了。

![](/images/2021/trojan-source-attacks/nqueens-error.png)

这个逻辑混乱，没有经过调试的为何能正确地输出？？？

![](/images/2021/trojan-source-attacks/code-error.jpg)

看着他的背影，我陷入了深深的沉思之中。

在DOS下，屏幕是80*25的，所以每一行只能看到80个字符。你看到的是：

![](/images/2021/trojan-source-attacks/nqueens-error.png)

这样子，实际上它是这样子的：

![](/images/2021/trojan-source-attacks/nqueens-cheat.png)

图片右边那行代码才是精华，“只要我的空格足够多，你就看不到我。”

所以即使现在有GUI了，你的IDE编辑器也一定要记得打开Word Wrap！

## Trojan Source attacks

https://www.trojansource.codes/trojan-source.pdf

> On October 18, 2021, Trojan Source attacks were issued two CVEs [43]: CVE-2021-42574 for tracking the Bidi attack, and CVE-2021-42694 for tracking the homoglyph attack. These CVEs were issued by MITRE against the Unicode specification.

Bidi attack影响大部分编程语言和代码库工具，比如这个代码，你在GitHub、GitLab、Visual Studio Code看到的是这样子的：

![](/images/2021/trojan-source-attacks/tsa1.png)

很清楚，调用了这个函数，资金被扣了50元。

，但是编译程序看到的是这样子的

![](/images/2021/trojan-source-attacks/tsa2.png)

他会忽略RLI，所以实际上在函数体里面执行的是; return，啥都没扣，就返回了。

当然，这个问题暴露出来后，Visual Studio Code立刻更新了Unicode控制代码的显示，类似

![](/images/2021/trojan-source-attacks/vsc.png)

估计很多代码审计工具也会对这种特殊的字符做提醒。

## 动手试一试1

你新建一个test.html，在里面输入

```html
''' Subtract funds from bank account then <bdi dir="rtl">''' ;return
```

然后用浏览器打开，会看到 

''' Subtract funds from bank account then return; '''

如果你这时候全选拷贝到其他文本编辑器，就是下面这行：

''' Subtract funds from bank account then ''' ;return

## Windows等显示文件扩展名不可信

为了安全，我们一般会推荐，不管任何时候，都需要显示Windows的扩展名，以免不小心点击到可执行文件，但是有RLO在，这一行为也不再安全了。

## 动手试一试2

比如你现在新建一个可执行文件“test.bat”，内容就写“calc”，（calc是系统自带计算器，无毒），然后重命名，光标停止在“.”前面，然后“鼠标右键”，插入“Unicode 控制字符”，选择“RLO”字符，再输入txt回车，则整个文件名就变成“testtab.txt”。

看看最终的样子：

![](/images/2021/trojan-source-attacks/rlo.png)

其实上面2个都是.bat可执行文件！！！但是很大概率很多人会认为下面那个是人畜无害的.txt文本文件。如果你双击这个“testtab.txt”，就会打开一个计算器。

这时候你可以试着发送到微信好友，他看到的文件名也是“testtab.txt”，如果在手机微信里面直接点开，可以预览到BAT文本，在PC微信点开，会打开所在目录，目录里看到的也是txt，隐蔽性非常高。

**当然，如果你谨慎得始终在显示文件列表时使用“详细信息”视图，并且关注“类型”字段和ICON，还是可以发现问题的。**

## Homoglyph function attack

trojan-source.pdf还提到一个同形文字攻击问题，看看这个代码：

![](/images/2021/trojan-source-attacks/homoglyphattack.png)

蓝色的H和红色的H是不同的字符，如果前面2个函数散落在庞大源代码的不同角落，你确定代码审计你可以审计出来？

