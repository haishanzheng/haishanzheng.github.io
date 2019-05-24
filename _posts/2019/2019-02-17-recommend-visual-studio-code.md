---
title:  "推荐好用的编辑器Visual Studio Code"
date:   2019-02-17 15:28:09 +0800
---

## 背景

Visual Studio Code以下简称Code。

**程序员的装备除了格子衬衫、狼爪冲锋衣外，就是Cherry、HHKB等键盘和一个好用的编辑器**。

Code是一个免费的开源的跨平台的代码编辑器，支持完全的定制，包括设置、布局、插件体系、快捷键等均可以完全定制，定制可以基于全局、Workspace、目录、项目等。

Code最开始是微软作为剥离过重的Visual Studio的一个在线版本Monaco编辑器，后来基于Electron框架演变成了客户端版本的Code。Visual Studio大家都知道，宇宙最强的IDE。微软内部有“Eating your own dog food”的传统，Code也是使用Code自己编写的，就类似编译语言的自举一样，是“By developers, For developers”的。

Code是在Erich Gamma带领下开发的。Eirch是谁？就是大名鼎鼎的Gang of Four (GoF)之首，《Design Patterns》的作者，十几年前你如果标榜自己是软件架构师，不飙几个设计模式都不好意思出去见人。他还写了JUnit，Eclipse Java development tools (JDT)，后来加入微软写在线的Monaco，后来GitHub出了Electron框架，原先是为另外一个编辑器Atom服务的，Code本身就是TypeScript/H5/CSS写的（你可以打开Help->Toggle Developer Tools看到所有的HTML代码。），所以顺理成章基于Electron推出了速度更快的客户端版本，再后来微软收购了GitHub。

Atom和Code我都有安装，Atom可以认为是个Editor，Code是个IDE，所以你安装完一打开就可以看到一个默认的暗色主题，最左边是系统自带和Extension提供的ICON视图切换，默认是Explorer项目树，中间是一个带tabs的编辑区域，中间下面是提示和调试、终端等窗口，右边一个minimap。

![](/images/2019/vsc/vscode.png)

Erich说IDE对某个语言的支持有三个级别：

- 第一级别是Syntax Highlighting，自动补全，这个级别只需要有这个语言的抽象语法树AST即可。
- 第二级别是IntelliSense, Go to Definition，Find All References等，支持代码跳转。这个级别不再只看单个代码文件，需要以整个项目来看。
- 第三级别是重构，为重构提供支持，这就需要对这个语言有更深入的了解，可以提供重构建议，自动化重构，重构完还保证可以更新项目所有的引用。

其他编辑器比如JetBrains家的ReSharper、PyCharm、PhpStorm、WebStorm等等，Visual Studio、Eclipse、Jupyter Notebook。这些是编程语言某个垂直领域的首选，每年几百美元的订阅费，在特定语言深耕挖掘，越贵越好（这些都有教育版免费，我也都有）。Code你可以认为是一个普遍适用的IDE。普适性的编辑器还有Atom、TextMate、Sublime Text、VIM、Emacs、Notepad++等等，如果你不想每个语言都安装一个编辑器，你就可以选择Code，很多项目本身也是多个语言混合的，所以使用Code也是一个更好的选择。编辑器的选择还有一个生态的考虑，哪个编辑器使用的人最多，愿意贡献插件最多，目前看起来Code是最好。

## 功能特性

- Basic：就是编译器他最基本要实现的功能，比如自动缩进、CRLF/LF切换，Tab转换成空格，UTF-8文件格式，Tab大小的定义。
- Extension：利用插件扩展编辑器的功能。扩展有市场，会根据项目作者或文件格式等自动推荐扩展。扩展市场可以按下载量和Star排序。
- IntelliSense：代码补全，参数提示，函数说明，方法列表。这也是非常重要的功能，减少了很多查函数参数说明的时间。
- 代码浏览：在项目内跳转，转到定义、转到实现、转到符号等等，由于MVC等分离技术的引入，有时候我们要完成一个功能，需要在后端代码、前端逻辑之间跳转，代码跳转使得思路不会被打断。
- 重构。
- 调试。Visual Studio的调试功能非常强大，Code也带过来了，如果是Python，可以使用ptvsd调试远程服务器的Python代码。
- 版本控制：与Git深度集成，在编辑器项目树可以看到文件的修改信息，打开文件左边也有，点击标记可以看到diff结果。如果安装GitLens插件，可以直接在代码看到哪段代码由谁在何时引入。当然，乌龟还是必须安装。
- Terminal集成。不离开IDE就可以运行脚本。
- Multi-root Workspaces。下面会单独介绍。
- snippets：代码片段，方便输入重复代码。
- Emmet支持，Code使用TypeScript开发，前端利器，Emmet必须支持。

![](/images/2019/vsc/emmet.gif)

- 其他：Extension提供的其他支持，比如多人在线共同编辑同一个代码Live Share。ScreenCast Mode等等。

## 如何学习

我这里只是抛个砖，我不打算写成帮助文件，Code有非常完善的帮助文件和Interactive Playground。如果你决定换到Code，应当花一周时间学习 https://code.visualstudio.com/docs 这里的所有内容，磨刀不误砍柴工，然后打印一个快捷键Cheat sheet放在显示器旁边，

![](/images/2019/vsc/vsccs.png)

在接下来半年内强迫自己使用到所有的快捷键。平时多对每个快捷键、功能、下载量超过几M的插件尝试，每个功能都不是随意存在的，如果你没用，那说明你一定是错了。而且技术是趋同的，即时今后换一个编辑器也能很快可以上手。

接下来我再说几个。

## 项目的组织形式

一般我们会有一些零散的文本文件需要编辑，有多个以目录组织的项目，在Code里是以Workspace来组织一个窗口的，所以你可以在一个Workspace打开零散的文档和多个Multi-root目录的项目，也可以打开多个窗口，每个窗口都是一个Workspace来隔离多个项目。Workspace本身也是一个JSON格式的文本文件，自定义设置可以应用在Workspace层或者Workspace内的项目目录.vscode层内。所以灵活性非常高。而且Workspace是会自动保持状态的，掉电后打开状态会恢复得一模一样，编辑器本身也支持自动保存。

![](/images/2019/vsc/navigate_editors.gif)

## Linting

代码静态分析后的提示是编程时非常重要的指导，可以减少很多运行时的错误，提高代码质量，包括编程规范也可以在这个阶段提示，比如Python的PEP8的要求，函数前面必须是2个空行，文件结尾必须是一个空行，换行对齐的规范，Markdown Lint的45条规则，包括比如heading #必须是顺序增加，多个heading不能字符串一样，如果写出不规范的代码（亲人两行泪），中间下部的PROBLEMS会即时显示提示信息。

## 所有可Filter

search功能非常强大，Ctrl+P打开文件支持搜索，支持全局搜索，左边Explorer也是可以搜索和键盘浏览的，Problems也是支持Filter的，设置项很多，跟Chrome、Firefox类似，也是可以针对某个关键字搜索的。

## 快捷键

熟悉一个编辑器实际上主要就是熟悉编辑器的快捷键，Code可以映射多个已有的编辑器的快捷键，比如Atom、Sublime Text等等，如果你那些编辑器快捷键用得比较熟悉了，你可以整盘复制过来。CLI一定比GUI快，键盘也是比鼠标快，一个好的IDE是你不用离开他就可以做很多包括编译、调试、运行的工作，一个好的编辑区域也是你不用鼠标完全使用键盘就可以快速完成编辑任务。举例如下：

代码一般都是以Line和Block组织的，在Code里，如果你在一行里，不需要选择整行就是默认以当前行操作，比如拷贝本行、移动本行、删除本行等。

以删除本行为例子，一般的操作你必须Home到行首，Shift+向下键全选本行，然后再Delete删除，而这些实际上只需要Ctrl+Shift+K即可完成。

Shift+Alt+上下键直接复制代码段也是类似。

Selection里你可以Expand Selection，会自动根据语言特点扩大选择范围，从Line到Block到函数到全部文本，然后再进行其他操作。

假如你要移动一段代码，传统的方式你需要选择整段代码，拷贝，到目的地再黏贴，但是这个实际上比较反人类，就像你在Photoshop时，你要移动一个图层的位置，你一定不是拷贝黏贴到某个(x,y)位置，你是激活M移动模式，然后通过上下左右键来移动，并在期间观察放在哪个位置最合适，Code也是可以这样的，当你选择了一个代码段以后，你可以按Alt+上下键移动，选择最合适的位置。

多点编辑也是很神奇的功能，你可以Ctrl+D选择相同的字符串，或者手动Alt+鼠标左键增加输入点，然后就可以多点编辑。

![](/images/2019/vsc/multicursor-word.gif)

还有块区域选择；Ctrl+/备注Line、Block；Ctrl+[/]调整代码段缩进。

还有比如你在一行中间，你要直接在下一行开始新代码，一般的做法是你按End跳到行末，然后按个回车，更快速的做法是直接按Ctrl+End直接跳。

熟悉了这些快捷键，会提高编码速度，**还会让自己有一种很牛B的迷之自信**。

## 代码浏览

由于现在代码越来越分散，同名的文件很多，需要在多个文档之间切换，这时候就可以使用Alt+左右键，回退前进，跟浏览器的“回退”、“前进”一样，锚点是游标的位置。还有语言的转到定义、转到实现等。我还安装了Bookmarks插件，一些比较常用的代码点可以加入Bookmark。

![](/images/2019/vsc/navigate_history.gif)

代码浏览还包括把整个文本代码段分析成为一棵树，可以查看定义的变量和函数，快速定位，折叠块等等。

## Python支持

Python语言的支持是通过Extension支持的，我自己是通过在Windows下安装Anaconda，使用conda隔离环境，Linting使用PEP8。

