---
title:  "Word的信息泄露"
date:   2024-05-28 11:20:15 +0800
---

特大号昨天发了一篇“串标翻车”的文章，里面有一段‘四、文件属性"出卖了"你’

> 两家公司应答文件文档属性中作者一致，存在串标行为
>
> 两家公司应答文件部分内容高度雷同，存在串标行为
>
> 两家公司应答文件的最后一次保存者一致，存在串标行为
>
> 两家公司报价文件标题均为“附件一：”，作者均为“王者”，存在串标行为
>
> 三家公司的采购文件格式、排版及服务方案内容存在高度异常一致，存在串标行为
>
> 两家公司报价表中格式、附图完全一致，存在串标行为

如果标书用Word来编辑，确实会泄露很多信息，而且很多人不知道。这个问题最大的其实是因为Word所见即所得的特性，隐藏了很多细节。

如果你默认安装Word后，不做任何设置，你很多信息会看不到。而且Word不像LaTex，或者Markdown，可以让你控制源代码编辑，你的文档有可能被塞入一些奇怪的东西。

如果你提交是打印后的版本，所见即所得可能不会泄露什么内容，但是如果你提交的是电子版就会。

## Docx实际上是个压缩包

如果你把docx后缀名改成zip，你就可以解压并且看到很多文档，以我做的论文模板为例：

![](/images/2024/inspectword/worddir.png)

目录很清晰，你所有的文本内容都在document.xml这个格式化文件里面，footer和header是页眉页脚设置，图片在word/media里，比如里面image1.png是校徽，image2.png是例子的图，这经常被我用来直接拷贝一个大文件里面的图片，或者PowerPoint文档里的图片。

因为是压缩包，就有可能有很多奇怪的东西。

## 我就打开Docx，打印一下，关闭，没有任何其他操作，Word为什么要提示我保存？

这个步骤看起来没有任何“变化”，对文档没有更新的操作，但是Word底层实际上做了很多其他工作。Word的页码是动态生成的，在Docx文档里面可以看到，他不会保存页码（可能会保存最后一次页码的缓存），所以有时候打开一个大文档，感觉会很慢，因为Word可能在重排整个文档的页码。

所以在你打开Word后，选择打印，因为你的打印机的设置，Word要重新计算很多值，打印完成后，整个文档实际上在底层已经变化了，所以他会提示你保存。

然而你一旦选择了保存，cp:lastPrinted这个值就会变化，并且最后保存着就会变成你。

这就是为什么打印店小妹要来背锅的原因。

## 隐写术

我们经常在影视作品里看到隐写术，古老的隐写术是使用特殊的墨水或染料。这些特殊墨水可以在某些条件下显示出来，例如在紫外线照射、加热导致温度影响、接触水分或其他特定的化学反应下显现。

现在是数字化时代，花样更多了。


## 利用Word对用户进行画像

你敢在简历里写，精通Office，只要给我一个你写的大文档，我可以全方位对你进行画像，给你打分。一般人分数不会高。

- 文件名都起不好的：标题一定要长，要在标题把信息写清楚，比如“论文标题-学号-作者.docx”。比如一个项目申请表，“项目名-学校名称-作者-日期.docx”，但是有些人他就是不注意，你给他“XX项目申请表.docx”，他会还给你一个“xx项目申请表.docx”。你下载下来再也无法分清到底是空表还是填写了内容的表。但是标题也不能太长，我们经常在微信里面看到一个文件，由于标题太长，微信显示不了太多信息，导致关键内容被隐藏的。比如“论文标题-学号-作者-v20240528(2)(3).docx”，这种的，“(2)(3)”是微信自动加的，但是最关键的版本信息v可能就看不到的。
- 内容：内容是最直观的，这也是大部分人都可以识别的。比如错别字，比如Word已经波浪线提示你语法不对，拼写不对还是不改的，比如专有名词大小写不规范的。
- AIGC痕迹未消除：把AIGC的内容不做任何修改，直接拷贝到文档。AI会写得很全面，也会有一些固定句式。
- 其他诸如不正确引用、抄袭。这个一般得通过查重工具才能查到。
- 内嵌Excel透视图，未删除原始数据。
- 审阅、批注没有清理的。你可以试着去办公自动化OA系统去下载一些文档来看看，可能就有很多过程审阅数据没有消除。

![](/images/2024/inspectword/review.png)

- 反推Word设置的：通过图片的清晰度反推出你的Word的设置，通过OOXML查看使用的Word的版本信息等。如果还在用doc文件，那Word版本一定非常非常老。
- 从互联网或者别的文档拷贝的：有些明显是从互联网拷贝的，Word会帮你加上“普通(网站)”样式。所以拷贝，一定要记得，永远使用纯文本黏贴。
- 查看文档属性：创建者，保存者，使用的模板。这个在我 OA模板里面有提到 [Word样式的使用和公文模板下载（最终版）](/2023/03/23/word-oa-template.html) ，会泄露模板目录位置。
- 直接格式：直接格式（Direct formatting）是指不使用样式，而是直接对单独的段落、单词、甚至字符应用格式。Word会自动收集整篇文档的所有格式，并且帮你显示出来，方便你对这些直接格式进行命名并在未来重用。一篇文档如果编辑得好，应该没有任何直接格式，只有命名样式。要查看是否有直接格式，可首先将样式窗格选项设置为“段落级别格式”“字体格式”均显示为样式，然后在样式窗格内观察样式名称为“xxx + xxx”的。下面这个图是一篇不规范的例子。

![](/images/2024/inspectword/style2.png)

- 是否正确使用样式、大纲、多级列表的。排版很乱的，这也是大部分人丢分的地方。

## 怎么最大限度地解决Word信息泄露的问题

除了前面提到的一些可能泄露信息的问题外，可以用新版Word自带的文档检查器来检查，可以解决很大一部分问题。

### 文档检查器、Document Inspector

> Word 文档可以包含以下类型的隐藏数据和个人信息：
>
> 批注、修订中的修订标记、版本和墨迹注释     如果已与其他人协作创建文档，文档可能包含修订、批注、墨迹批注或版本的修订标记等项。 此信息可以让其他人查看处理文档的人员的姓名、审阅者的评论和对文档所做的更改，以及你可能不希望在团队外部共享的内容。
>
> 文档属性和个人信息     文档属性或元数据包括有关文档的详细信息，例如作者、主题和标题。 文档属性还包括由 Microsoft 365 程序自动维护的信息，例如最近保存文档的人员的姓名和文档的创建日期。 如果使用了特定的功能，您的文档还可能包括其他类型的个人身份信息 (PII)，例如电子邮件标题、请求审阅信息、传送名单和模板名称。
>
> 页眉、页脚和水印     Word 文档可以包含页眉和页脚中的信息。 此外，你可能已向 Word 文档添加了水印。
>
> 隐藏文本     Word 文档可以包含格式化为隐藏文本的文本。 如果不知道文档是否包含隐藏文本，可以使用 文档检查器 进行搜索。
>
> 文档服务器属性     如果文档已保存到文档管理服务器上的某个位置，例如文档工作区网站或基于 Windows SharePoint Services 的库，则该文档可能包含与此服务器位置相关的其他文档属性或信息。
>
> 自定义 XML 数据     文档可能包含在文档本身中不可见的自定义 XML 数据。 文档检查器 可以找到并删除此 XML 数据。

![](/images/2024/inspectword/inspectdocument.png)

图中的“上次修改者”guest是我，为了避免麻烦，我的Word配置我的名字就是guest。

但是要注意下面：

> 注意: 此检查器不能检测使用其他方法隐藏的文字（例如，白色背景上的白色文本）。
>
>注意: 该检查器无法检测被其他对象覆盖的对象。

## 样式名的清理

![](/images/2024/inspectword/style.png)

以我OA的模板为例，我就加了一个没用的样式名“v20230409-样式版本”。

前几年我在阅读一个厂商输出的报告，在样式里面发现了另外一个厂商的名字，一问，才知道，这个厂商创始人是从另外那个厂商毕业的，沿用了人家的模板。
