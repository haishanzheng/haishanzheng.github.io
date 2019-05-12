---
title:  "推荐使用FeedDemon访问blogs"
date:   2003-07-08 23:43:55 +0800
---

关注的blog多了，我们经常记不起好朋友的blog地址。有时把好友的blog加入了收藏夹，可是经常忘记访问，某日想起来了才点击收藏夹一个个去点击，又被堆积起来的内容淹没。如果能有一个软件能自动帮我们检查好友的blog，如果有新的内容就自动提醒我，那该多好呀？（昙花一现的Push技术？）  

我很幸运地告诉你，已经有了，还很多，而且大部分专业的blogger都已经在使用。我现在跟你推荐其中的一款FeedDemon。使用FeedDemon，你可以随时掌握好友的动态又不耗费任何力气。正如我一直强调的，blog跟其他主页的区别是blog注重的是内容和时效，网页只是表现形式，blog的主要内容是一篇篇文章，所有的显示模式都是垃圾，一般blog系统都会把blog内容使用遵循RSS标准的XML格式保存起来，这样通过更改XLT，你就可以用IE，手机，PDA等等东西阅读他们。符合RSS标准的XML文件记录了blog最新发布的几条文章的标题，简要内容，发布时间，发布人等等。通常有提供以上提到的RSS XML文件的blog都有一个这种![](/images/2011/tech/xml.gif)图标。FeedDemon就是一款在Windows下自动获取blog XML文件并集中显示的软件。关于更多RSS的信息请查看其他链接。  

  1. 下载[FeedDemon](http://www.feeddemon.com/)的[beta pre-release](http://www.bradsoft.com/feeddemon/beta/index.asp)。

  2. 下载后双击FeedDemon-beta1a.exe安装。

  3. 安装完毕点击应用程序图标打开他，对于任何一个新软件，我推荐第一次使用首先查看这个软件的所有菜单项并配置参数，配置完参数关闭软件，给软件保存设置的机会，第二次才开始正式使用。

  4. 首先配置，点击File->Options...，这里设置比较简单，首先是General，选择Auto update。接着是System Tray，我的选择是每次最小化逃到System Tray，如果有新信息随时通知我。Defaults里面Auto-update frequency时间不要设置太短，每半天访问一次就好了，我的设置是300分钟，不要设置的太频繁，加重人家blog服务器的负担呵呵。其他参数可以按自己的需要设置。

  5. 接着建立我们关注的blog的组，点击File->New->New Listing...，建立一个新的组“我的每日关注”，我们将把我们关注的组全部放入这个Listing内，方便管理。

  6. 接着开始添加blogs，首先使用IE浏览某个blog，找到![](/images/2011/tech/xml.gif)这个标记，鼠标右键点击这个图标并选择“复制快捷方式”，接着到FeedDemon，在“我的每日关注”里点击New->New Channel...，Next，FeedDemon会自动把剪贴板里面的URL拷贝入URL输入筐，再次Next，自动找到这个blog的titile，继续Next，一个blog就被添加进来了。

  7. 好了，继续添加其他blog，把FeedDemon加入启动里，这样你每日只需用FeedDemon看看各个blog的新闻，每隔一段时间再上他的网站看看，这样既不会累，也不会漏了什么重要信息。

FeedDemon目前还在测试中，所以暂时免费。  

thx hwTony。。。  


## 评论

*****
**匿名** 在 *2004-04-30 11:47:34* 说: 能不能推荐几个比较好的频道？
建完之后都是一些英文的站点，看了就头痛

*****
**匿名** 在 *2003-07-14 13:09:35* 说: 下载链接是不是出错了?下不了  e文的  

*****
**郑海山** 在 *2003-07-09 15:55:10* 说: hehe，给图片叫上全名就可以了。你可以看到我以前的图片是src=/images/aritclesimage/，现在都换成src=http://dog.xmu.edu.cn/images/articlesimage了。就是这个原因。。。

*****
**匿名** 在 *2003-07-09 10:46:07* 说: 在feedemon中，可以看到你的文章里的照片，
而我的blog却看不到(http://www.lvye.org/~leo/b2/), 是什么原因？

