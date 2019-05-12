---
title:  "有了相机你还需要：使用Picasa管理照片，使用数码相框看照片 "
date:   2010-04-29 11:32:38 +0800
---

##前言



你可能会记得我前段时间写了 [入手sony 2010最新卡片机DSC-TX7C，测评，TX7C，TX7]({{ site.baseurl }}/2010/02/08/dsc-tx7c.html)，拍了照片要管理吧，所以我又写了这个。



##照片管理软件选择



买了相机，照了很多照片，得找个工具来管理，当然你可以用Windows自带的文件夹和图片浏览器，简单方便，这也是一种方法，不过效率比较低而已。我的TX7C sony自带的PMB实在很傻帽，虽然他的日历功能很华丽，但是导入照片目录名选择功能太弱，浏览照片不支持快捷键，简直不是人用的。试用了ACDSee Pro 3和picasa 3.6后，决定使用picasa来管理照片。ACDSee Pro 3将近200美元，功能可谓强大，适合专业级，对于我这种非专业用户，picasa功能已经足够。曾经非常bs acdsee，认为他好好地做看图软件就好了，以前的acdsee3.1非常受推崇，又小速度又快，是看JPG（JaPanese  Girl）文件的最佳选择，是安装系统的必备软件，何必搞得现在300多MB，好大的棉花糖，直到我使用了才知道，我错怪他了，用来管理照片很赞。



首先小比较一下。

  * picasa界面美观，平铺的文件夹切换效果非常cool。而且略缩图显示也很漂亮。acdsee pro界面黑色调为主，中规中矩，显示信息非常齐全。文件，EXIF，IPTC，XMP所有信息都很齐全。
  * picasa地理标志结合google earth，可以自己编辑。acdsee可以显示gps数值，不知道是否可以调用地图来显示。如果我说不知道，那基本上可能就是不能。
  * picasa有头像检测功能，可以按头像分类，这样子可以方便查找和统计。比比看你们家谁最爱拍照片。acdsee pro没有。如果以前没有整理过头像识别，那整理一遍还是蛮累的。
  * acdsee pro搜索整理功能异常强大，一张照片你至少有category，rating（支持5星，picasa只有星标和非星标），tag（checkbox，跟picasa星标一样），keywords等，而且可以save search，所以用acdsee可以把照片管得死死的。picasa相对较弱。
  * acdsee pro照片后期修饰功能异常强大，picasa一般。
  * 保存整理信息的方案2者有较大不同，各有优缺点。picasa会把你给照片的命名和tag信息直接写入照片，然后在各个照片目录的.picasa.ini内写IPTC不支持的比如星标，头像等信息。头像和相册存放在系统目录。提高浏览速度的略缩图，star cache也都放在系统目录。可以说是一团糟，既有修改原始照片，又有自己的数据库。如果你有情结，你可以考虑使用acdsee pro，acdsee不会去修改原始图片，他有独立的数据库，而且数据库支持导入导出。他也支持把数据库信息写入到图片，需要点击一个菜单。保存方案这个功能非常关键，因为我们通常不止1台电脑，我们要在电脑之间共享图片和图片信息，接下来会讲如何做到多台电脑同步。
  * picasa和acdsee pro之间可以共享数据的是IPTC，不过acdsee pro把picasa的IPTC信息获取后写入XMP，就不再理会IPTC数据了。所以如果你在picasa修改了数据，在acdsee下打开会发现无法同步过来。
  * picasa web相册功能较强，部分地区有可能被盾而无法使用，好可惜。

![](/images/2011/photomanagement/picasa_156339_folder_list.gif)

具体你选择哪个需要你自己做测试，当然我推荐Picasa不是说他们2个要争得你死我活，你用Picasa管理照片不妨碍你用AcdSee到照片目录里面去浏览。我要说的是你要记住这张照片好，你可以记在脑子里，文本文件里，我只是推荐记在Picasa内而已。



我一般是建立一个Photo目录，再一个PhotoOther目录，Photo纳入picasa管理，PhotoOther放不想在picasa管理的照片，再一个PhotoSync目录，在有多台电脑需要同步时临时存放文件。导入照片后查看照片，做些修改，加些文字说明，标记些star照片。如果需要给几张照片给别人或者打印台历，可以新建立一个相册，然后把照片加入相册再导出。



##如何在不同电脑之间同步照片



现在每个人有几台电脑是很平常的，办公室1台，家里1台台式机，1台笔记本，如何在这些电脑之间共享照片，使得即可以快速查看，又可以共享对照片所做的标记等信息是个大问题。我在这里推荐一个模式，rsync，不是很完美。用过linux的一般都知道rsync，rsync是用来文件同步的，可以同步某个目录里面的所有文件，而且同步只传输更改的文件，速度非常快。我的方法是，以某台作为服务器，通常这台有固定IP，所有的3台电脑照片目录全部放在D:\Photo，目录名一致才能在每台电脑打开picasa设置都一样。简单起见1台作为输入，所有的照片都从这台导入，然后更新到服务器，其他的电脑再从服务器同步下来。如果要更改照片，添加标记，star等也只在输入的那台，这样子才能保证其他几台不会不小心互相覆盖。如图：

![](/images/2011/photomanagement/2010-4-29-8-28-02-cwrsync.jpg)

为什么不完美，因为picasa的数据库不是记录级的，所以无法在各个不同客户端编辑后更新上去。而且为了防止不小心同步出错，使用了单向同步。你可以选择不删除服务器上输入客户端没有的文件，这样子可以做到多个客户端一起上传，但是无法在服务端删除客户端删除掉的照片。



同步必须同步3个目录，一个是照片目录，通常是D:\Photo，一个是picasa数据库，是 C/Documents and Settings/Haishion/Local Settings/Application Data/Google/Picasa2/ ，一个是picasa相册，C/Documents and Settings/Haishion/Local Settings/Application Data/Google/Picasa2Alumni/ 。如果你使用win7，你可以使用mklink命令行，把3个目录整合在一个目录内一个同步语句即可。



附带rsync配置。



服务端


[photo]

path = /cygdrive/D/Photos/

read only = false

auth users = photo

[picasa2]

path = /cygdrive/C/Documents and Settings/Haishion/Local Settings/Application Data/Google/Picasa2/

read only = false

auth users = photo

[picasa2albums]

path = /cygdrive/C/Documents and Settings/Haishion/Local Settings/Application Data/Google/Picasa2Albums/

read only = false

auth users = photo



客户端



注意win7可能需要修改以下设置：

SET CWRSYNCHOME=%PROGRAMFILES(x86)%\CWRSYNC



同步命令，这是往上传的

rsync -vzrt --progress --delete /cygdrive/D/Photos/ photo@xxx.xxx.xxx.xxx::photo < password.txt

以上命令只做参考，注意--delete，rsync功能很强大，破坏力也很强，注意多测试，如果不小心把一个空目录同步到c盘会删除整个c盘数据咯，这个不关我的事，请先做好备份。



##关于视频



以上说的是图片，视频不建议放在Photos里面，因为视频一般都很大，会妨碍图片刻录光盘备份。建议视频是单独放出，使用 [会声会影X3].Corel.VideoStudio.Pro.X3 等编辑软件去掉不要的画面，再保存成mp4格式，.h264视频压缩，AAC或者mp3音频压缩，每秒30帧。保存后的mp4可以放入同步目录。如果源视频是avchd格式的，比如我的TX7C生成的，你需要安装ffdshow和Haali Matroska Splitter才能观看，一般整合解码包内都有。



##打印和数码相框



照片拍了这么多，不看=没有，所以做屏保，win7小工具，打印台历，买个数码相框都是不错的选择。打印可选择3R（5寸），4R（6寸）稍大，不要裁，留边。做台历的网站很多，比如可牛影像，光影魔术手，网易印像派等。最推荐的是买个数码相框。你还记得你家里厚重的婚纱照相册，一辈子难得看几次，买个数码相框这个问题就可以解决。目前数码相框基本上已经可以接受，只要是7寸，分辨率在800*600或者800*480均可，个人觉得7寸就ok了，太大了也不好看，就不是相册而是屏幕了。sony的2010年4月新品DPF-D75 taobao价在6xx左右，过段时间会再降，效果非常好，其他大厂相同价位的也可。国产的几个我也观察过，做工跟sony不是一个等级的，价格便宜，部分还有带mp3，mp4播放功能，拜山寨厂商，但是做工非常烂，有人说金玉其表，败絮其里，如果是败絮其表，更不知道里面是什么了。所以多花点钱买个好点的寿命久一点，是否有声音意义不大，相册有声音还是蛮奇怪的，注意最好附带遥控器，这样子一帮人一起欣赏不会很别扭。分辨率和屏幕表现是最关键。内存容量意义不大。一般都可以自动开关机，每天开12小时一年的耗电在15元以下。数码相框相比打印的策略好太多了，画面更鲜艳，台历一个月才能看一张照片，而数码相框时时在更替。切换效果动感很吸引眼球，日历永远是最新的，不需要抛弃。但是问题也是有，要拖着电源线，家里停电没事干没办法点蜡烛看。

![](/images/2011/photomanagement/t2pgfcxandxxxxxxxx_382947949.jpg)

![](/images/2011/photomanagement/t2-abcxbldxxxxxxxx_61179837.jpg)

##延伸阅读



  * http://picasa.google.com/support/bin/static.py?page=guide.cs&guide=16027 Picasa 3.6 使用入门指南
  * http://en.wikipedia.org/wiki/Extensible_Metadata_Platform The Adobe Extensible Metadata Platform (XMP)
  * http://www.itefix.no/cwrsync/ cwrsync - Rsync for Windows
  * http://www.reelseo.com/basics-web-video-file-formats-video-containers/ The Basics of Web Video File Formats and Video Containers
  * http://www.sonystyle.com.cn/products/dpf/index.htm sony 数码相框
  * http://yxp.163.com/ 网易印像派

## 评论

*****
**dogbreedinformation** 在 *2010-12-03 13:25:18* 说: 好久没看你更新了，最近很忙？
楼主对picasa情有独钟哦!

*****
**美文** 在 *2010-09-26 12:58:47* 说: 网站挺不错的，风格很喜欢。能换了链接交个朋友不？
墨客草屋：http://www.meiwenzhang.com/blog

*****
**moongirl** 在 *2010-07-24 15:28:32* 说: 现在不便宜哦!不知还要多久,才能大众化!

*****
**welder** 在 *2010-07-20 16:05:21* 说: 以前看过你关于Microsoft Money的介绍，这次偶尔来看，没想到还在继续Blog。

而且还有JPG（JaPanese  Girl）的妙论，呵呵。

