---
title:  "我所使用的29种或开源、或免费、或正版化软件"
date:   2024-01-18 15:11:33 +0800
---

我一般会定期将操作系统进行重装，[重装系统的意义](/2019/02/12/the-meaning-of-system-reinstalling.html)。寒假到了，正好重装了操作系统，所有软件都重新安装了一遍，分享出一些我还保留下来的软件。

软件的变化是永恒的，我的博客推荐过不少软件，但是很多都消失在时间的长河里。在我推荐的时候，看它啥都是好的，没有缺点，非它不可，在它消失的时候，我不得不手工或者编程来将它的数据转出到另外一个它。但是我相信，**在那个时间点，确实它是最合适的。正如现在下面推荐的这些软件一样。**

如果你有更好的也可以推荐给我。

我去年也写过，[高校正版化软件实施过程中的一些问题](/2023/07/01/genuine-software-in-university.html)。一个软件即使付费，也不能保证他一定可以活下来，而且还可能阻挠你迁移到另外一个软件。还有就是手机端买软件或买次时代游戏时，如果这个软件或游戏不是多平台的，会导致你迁移到另外一个平台的成本很高，你很难抵抗供应链断裂的风险。

在我推荐的软件里，很多都采用了插件的架构，比如Edge、Obsidian、Anki、VSC、KeePass等等。插件使得软件的扩展性更高，软件的可配置项可玩性增加，但是也带来了更多的安全风险，就像开源软件一样，由于底层依赖复杂，你必须信任很多人，有了插件体系，你要信任更多的人。所以插件的选择也是很重要的，使用尽量少的插件，并且永远保持一个可能已经被黑的“底线思维”。

## 选择的理由

我选软件一般会遵循以下几条原则：

- 销量高，一直有活跃更新的。历史悠久的。
- 或后起之秀。
- AIO和独立都会考虑，但是会尽量选择体量小的，体量越小，越专，一般会在那个垂直赛道跑到第一。
- 优先从下面顺序选择：开源、免费、收费的免费版。
- 供应链安全，尽量选择有跨平台能力的软件。未来可能得从x86、ARM架构转出，或者更换操作系统，如果能有源代码在，转换会比较容易一点，或者至少对数据的导入导出会有代码可参考。
- 如果有数据存储的，应当格式简单，方便迁移的，牺牲一点功能性，提高自主可控能力。

## 笔记类软件

### Obsidian

双向链接笔记类软件，底层是Markdown文件格式存储，迁移方便。我用Obsidian替换了用了十几年的OneNote。OnoNote它是全平台免费的，但是体量太大，客户端2016年后不再更新，否则OneNote是一个非常合适的选择，一个分区就是一个文件，一个笔记本就是一个目录，可导出Word。

同类的还有Logseq、Notion、Foam等等。

![](/images/2024/software2024/obsidian.png)

### 有道云笔记

和Obsidian组合，只用来记录不敏感，需要跟手机同步的笔记。

### Zetero

文献和网页收集整理。以前我用NoteFirst。如果有印象，我在2004年还推荐过网博士。

![](/images/2024/software2024/zetero.png)

### Anki

记忆卡片，可用来背单词、题库等任何需要记忆的内容。算法参考自Hermann Ebbinghaus的遗忘曲线，十几年前我用SuperMemo。现在也有专门给学生使用的实体的电子纸学习卡，比如喵喵机等等。

![](/images/2024/software2024/anki.png)

![](/images/2024/software2024/miaomiaoji.png)

### Freeplane

思维导图。同类的很多，比如FreeMind、Obsidian Canvas、VSC插件、Mermaid等等。[内容和格式分离，说说写论文、写文档、画框图的事](/2018/05/03/split-content-format.html)。

![](/images/2024/software2024/freeplane.png)

### draw.io

拓扑图工具。可看 [等保的定级网络拓扑图怎么画？用什么画？](/2021/09/27/netwok-topology-diagram.html)

### PDF Arranger

PDF应当是一个输出格式而不是一个编辑格式，任何时候要更改一个PDF内容，理论上应该是编辑源头，并重复输出。PDF就类似已经打印出来的A4纸，你可以在上面描描画画形成花脸稿，最终还是要重新在Word里编辑后输出。

一般人会遇到以下几个PDF场景。

- 阅读：Chromium内核的浏览器比如Edge、Chrome已经可以直接打开PDF，做标注等等。笔记类软件一般也可打开PDF标注。
- 生成PDF：LibreOffice、Word可以生成PDF。
- 简单编辑：比如修改元数据、对页面进行调整、抽取页面、合并等等。我用PDF Arranger。
- 复杂编辑：我用得不多。因为PDF是输出格式，所以编辑还是比较麻烦的。

![](/images/2024/software2024/pdfarranger.png)

## 多媒体类

### EV录屏

对屏幕进行录制很方便。

![](/images/2024/software2024/ev.png)

### Potplayer

看电影。

![](/images/2024/software2024/potplayer.webp)

### foobar2000

听些本地的音乐文件很适合。没有广告，不会分析你的习惯，扔一些文件，建几个播放列表，随机播放即可。

![](/images/2024/software2024/foobar2000.png)

### ShareX

截图工具。

![](/images/2024/software2024/sharex.png)

### ScreenToGif

制作GIF动画。

![](/images/2024/software2024/screentogif.gif)

### GIMP

Photoshop平替。

## 系统类软件

### 7-Zip

虽然Windows11已经支持Zip和RAR等压缩格式，但是对RAR的支持还不是太完善。用了蛮久。

### Everything

快速定位全盘文件和文件夹，由于建立了索引，速度非常快，比系统自带的快，特别是现在微信文件夹很大的情况下，用它来找文件速度非常快。

### FileZilla

FTP下载工具。FTP服务器已经较少了，部分人还是需要的。

### KeePass

密码管理软件。[关于密码的一些事](/2019/08/05/all-about-password.html)

### TreeSizeFree

分析磁盘空间占用的工具，扫描全盘非常快，可以定位占用比较大的目录。

![](/images/2024/software2024/treesizefree.png)

### CurrPorts

分析程序开的端口的工具。

![](/images/2024/software2024/currports.gif)

## 开发类

### Visual Studio Code

Visual Studio Code是我的主力文本编辑和开发工具。[推荐好用的编辑器Visual Studio Code](/2019/02/17/recommend-visual-studio-code.html)

### DBeaver

数据库管理工具，支持MySQL等等多个数据库产品。支持SSH隧道连接。

![](/images/2024/software2024/dbeaver.png)

### XShell

远程SSH连接工具。[推荐中过木马的SSH客户端工具Xshell](/2017/09/18/ssh-client-in-windows-xshell.html)

### Putty

老牌的远程SSH连接工具，只有一个EXE文件，很小，一个很好的补充。

### Tortoisegit

Git的GUI工具，可结合资源浏览器右键菜单，虽然VSC也支持操作Git，但是一个独立的GUI有时候也很方便。

## 备份类软件

### Macrium Reflect Free

备份是非常重要的，如果数据没有2份以上的备份，可以认为这个数据是不存在的。MRF支持对整盘在线进行备份。[就把备份做好有多难](/2017/02/04/backup.html)

### FreeFileSync

FreeFileSync我用在离线备份，定期接上移动硬盘，打开同步方案，同步一下。

FreeFileSync可以保持2个文件夹同步，支持本地文件夹、网上邻居、FTP。文件比较根据文件更新时间、大小（1T几分钟就可以检查完）或者内容（很慢，没试过）比较。可以后台自动同步。带简单的版本管理。可以建立配置文件，保存多个同步方案。使用非常简单，选择2个目录，检查不同，同步即可。

所以FreeFileSync就是一个copy的功能，你可以认为他就是一个增强型的copy。为什么不用copy？拷贝黏贴也很简单，但是对于大量文件拷贝黏贴不靠谱，无法断点续传，遇到锁定文件可能出错。拷贝黏贴只适合小量文件，习惯好的人一般会拷贝完检查一下源和目的文件夹的文件数量和大小。

[免费个人数据备份软件介绍：FreeFileSync、Syncthing](/2018/04/14/intro-freefilesync-syncthing.html)

### Syncthing

Syncthing我用在在线远程备份。Syncthing也是同步2个文件夹的，类似私有云的Dropbox，支持多台客户端实时双向同步，但是他不支持本地文件夹，只支持远程。

### 群辉类NAS

NAS类的建议家里一定要有一个。
