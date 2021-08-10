---
title:  "xbox，XBMC，MC360，mfxstream，搭建xbox娱乐平台"
date:   2008-08-25 17:04:29 +0800
---

XBMC全称Xbox Media Center，http://xbmc.org/ http://xbmc.org/ ，是xbox破解后的必装软件，通过安装XBMC，替代微软自带的launcher，可以为xbox扩展很多新的功能，让濒临死亡的xbox继续为我们发挥余热。XBMC可以显示硬盘上的游戏，音乐，视频，图片，可以读取共享服务器上的影音资源，共享方式可以是：ftp，xbms，samba，upnp等等。一般人都会选择samba即网上邻居模式为xbox共享影音，我这里要介绍的是使用xbms协议搭建共享平台，为没有samba环境的用户提供另外一种简单的解决方案。 

![](/images/2011/other/xbox1.jpg)
![](/images/2011/other/xbox2.jpg)

xbms是专门在xbox之间使用的一个简单协议，功能比较简单，只有基本的列出文件目录，返回文件信息，读取文件内容3个大的功能，而这就已经足够，相对于ftp的复杂，samba的安全性，xbms显得小巧好用，xbms只需开放一个端口即可在双方之间传递数据。搭建xbms服务器可以使用ccxgui，mfxstream等工具，在我测试过ccxgui，不知道为什么ccxgui搭建的服务器在最新版本XBMC下首次打开一个影片速度非常慢，所以转而使用mfxstream，mfxstream是开源软件，对中文编码支持不是太好，幸好是开源的，所以只需下载源代码对下面代码修改即可很好的支持中文文件名。 _mfxstream\Src\XBMSPUtility.cs _ _86 strFolder = System.Text.Encoding.GetEncoding("gb2312").GetString(pConn.GetMessage, 13, nFolderLength); // strFolder = ByteArrayToString(pbResult, 0, nFolderLength);_ _380 pbFileName = System.Text.Encoding.GetEncoding("gb2312").GetBytes(msgFileName); // pbFileName = StringToByteArray(msgFileName);_ _pbFileInfo = System.Text.Encoding.GetEncoding("gb2312").GetBytes(msgFileInfo); // pbFileInfo = StringToByteArray(msgFileInfo);_ 当然，以上修改必须在XBMC配置了中文的环境下使用，不要看这个修改很简单，字符串硬编码很难看，花了我一天时间。。。 以下顺便介绍下如何安装XBMC并且如何让他支持中文。 

  * xbox必须破解
  * 下载XBMC，由于版权的原因，xbmc官方网站不提供xbox版本的二进制下载，我们可以到T3CH站点下载最新的SVN版本XBMC，http://t3ch.yi.se/ http://t3ch.yi.se/ 
  * 把解压后的XMBC目录ftp传入xbox，并设置菜单项，开机直接启动的请另行查找方法。
  * 下载皮肤MC360，可以把xbox显示模拟成xbox360。http://blackbolt.x-scene.com/ http://blackbolt.x-scene.com/ ，放到XBMC\skin下。
  * 拷贝c:\windows\fonts\simsun.ttc，放到XBMC\media\Fonts目录下，并替换名字为Arial.ttf。这样省得修改skin文件信息。
  * 设置XMBC让他显示中文，修改以下几个位置。 System->Appearance->Look and feel 选择 skin->mc360 skin fonts 选择 Arial System->Appearance->region->language -> Chinese (Simple) charset选择Chinese Simplelified (GBK) 如果设置后乱码，尝试重起，修改分辨率等方法。
  * 开启mfxstream服务，添加路径。
如何使用 mfxstream http://mfxstream.sourceforge.net/tutorial.html http://mfxstream.sourceforge.net/tutorial.html  在XBMC添加source时输入类似 xbms://user:password@192.168.1.2:1400/ 。 如果你没有xbox也想体验XBMC也可以，去官网下个windows版本的XBMC，视频播放的内核是mplayer。

