---
title:  "使用AcidImage做Palm上的电子相册"
date:   2003-04-18 18:43:05 +0800
---

AcidImage看图速度很快，支持GIF，PGP，BMP，JPG，SplashPhoto等等格式，可以说是Palm上最好的看图软件，而今天我说的重点不是如何用他来看图片，我们要用AcidImage来建立自己的电子相册。  
数码爱好者一般都会有台dc，用dc照了很多照片，如何能够做到自己可以时时欣赏或者给家里人分享就是个问题了，如果你buy一台Palm，这问题就迎刃而解了。Palm的图片显示效果完全可以跟笔记本媲美，个头可比笔记本小多了。我这里推荐一种我用NR70配合dc做的电子相册，涉及到如何管理，如何使用软件，如果你觉得别的方式更好，请写信告诉我:)  
电子相册的大致功能就是：可以略缩图显示，可以幻灯片播放，最好还能分类。在做电子相册之前，先来学习学习AcidImage的基本用法。  

原创by沈晶冰  

这是AcidImage官方网站http://www.red-mercury.com/acidimage.html上提到的feature：  

  1. 无需转换  

  2. OS5的机器使用ARM内置的JPEG解码，速度飞快  

  3. 支持hires，hires+  

  4. 在某些CLIE型号上有硬件加速  

  5. 支持群组管理  

  6. 图片可以放在卡上任何目录  

  7. 略缩图显示（hires+可达到每次显示30张）  

  8. 隐私支持，可以隐藏图片，看xx图片更加方便  

  9. 可同时工作在灰度机器上。。。你还在用灰度机？扔了吧。。。  

  10. 快速缩放图片，最高可达放大1600%倍  

  11. 使用幻灯片功能同时观看多张图片  

  12. 在hires+上可自动旋转图片以适应屏幕  

  13. 可直接观看数码相机拍下的图片，图片太大也没关系，只要你有足够的RAM空间，可以全部显示，如果空间不够，AcidImage可以自动载入小尺寸预览。载入一个1024*768的JPG图片需要差不多1.5MB的空间  

看看ScreenShot  

![](/images/2011/acidimage/pref.gif)  

Preferences里参数设定的含义  
Show Startup Title Screen：是否在启动AcidImage时显示Title Screen，可以去掉。  
Show Corner Tools：在查看图片的时候显示功能按钮，这个可以保留着，缺点是占了四个小地方。  
Load JPEGs Full Size：jpeg不管如何都使用全部载入，选上速度会变慢。  
Lock Edges When Panning：锁定，用Pen移动的时候图片不会移到屏幕外面去。  
Save Thumbs：是否在卡上保存略缩图，这样AcidImage会在相同的目录里面生成AIThumbI.thm和AIThumbD.thm2个文件，推荐选上，可以加快预览速度，缺点是多了2个垃圾文件。。。  
View Progress Meter：如何显示打开图片时的进度条，一般full模式。  

![](/images/2011/acidimage/imgtype.gif)  

支持的Image类型，AcidImage会在打开的时候查找卡上所有包含这些文件的目录。我只把JPEG选上，希望能提高一点点速度。  

使用AcidImage之前，先要在脑里面建立这么一个概念，我们目标是为了看到图片，怎么看到图片，怎么看到就有多种方式，可以一下子把全部图片显示出来让你选一张看，可以把卡上的文件系统显示出来让你自己找，而AcidImage采用了一种折中的方式，只把有图片目录选出来，然后你自己再进入选看。所以就有3层画面4种画面形式，因为中间2种画面形式属于同一层的不同显示。第一层显示卡上所有有图片的目录，点击进入第二层显示目录里面的图片列表，点击图片才真正显示图片。把这个概念模式理解清楚了再往下看:)  

![](/images/2011/acidimage/level1.gif)  

这是打开AcidImage时第一次进入的画面，AcidImage会自动扫描卡上所有包含图片的目录并把他列出，扫描时窗口上面会有一个小卡的图片在一直闪动，每个目录项都有一个文件夹打开关闭的标志，一个CheckBox，选或不选可以批量操作。后面就是目录的名称。缩进的表示是子目录。（这里有一个小技巧，等下讲到）  
最底下是一个SlideShow和Open。Open就是打开目录了。  
如果按在目录项并停留一段时间，出弹出一个快捷菜单，如下：  

![](/images/2011/acidimage/tap.gif)  

SlideShow Checked是使用幻灯片模式播放被选上的目录。  
Delete Checked是删除被选上的目录。  

如果点击SlideShow，则会弹出一个筐，要求设定SlideShow的选项，设定完毕就可以使用幻灯片模式查看了。  

![](/images/2011/acidimage/slidepref.gif)  

Delay是2个图片之间间隔多长时间，Loop是是否播放完了从头再来，Random Order是否使用随机模式播放，Tiny Progress Meter是在图片载入时会在图片的最上面显示一个非常小的滚动条。  

如果在目录上点击Open就进入下面这个画面，这个画面有2种模式，一个是List模式，我们称为列表模式。一个是Thumb模式，称为略缩图模式。  

![](/images/2011/acidimage/level2list.gif)  

列表模式显示图片的位置，类型，图片名和建立时间，可以排序，可以同时操作多个图片。列表模式图片如下：  
最上面一行是排序按钮，L是Location，T是Type，Name是文件名，Date是文件建立时间。点击排序。  
CheckBox用来快速操作多个图片。同前面的目录画面，Tap并且停留一段时间会弹出菜单。  

![](/images/2011/acidimage/tap2.gif)  

![](/images/2011/acidimage/level2thumb.gif)  

略缩图模式可以预览多张图片，可以设定每次预览的图片个数，有1，4，20几个数量可选。  

直接点击图片就打开图片了。  
正在打开图片时的ScreenShot：（View Progress Meter为Full时的样子）  

![](/images/2011/acidimage/open.gif)  

底下显示经过了几秒，还剩几秒。  

![](/images/2011/acidimage/view.gif)  

打开后，上下左右共有4个按钮，左上角是隐藏这4个按钮，隐藏了但是功能都还是在。如果隐藏了点击左上角又会显示出来。右上角是关闭，不是删除咯。左下脚是切换缩放的工具栏。右下角是旋转图片。这是按了左下角后的显示，100%是按图片原始大小显示。左边是Fit Screen显示。  

Group模式，就是在卡上再建立一个逻辑分类，分类可以是中文名。由于用处不是太大，这里就不介绍了。  

有了以上的知识，可以跟着我来做一个电子相册了，一步步都要做对咯。  

为了可维护方便，推荐在电脑上做一个目录专门放相册，改动只在这个目录里面修改，如果做了变动，再拷贝覆盖到卡内。由于是照片，所以一般只需用JPEG。  

  1. 首先我们把自己所有用数码相机照得的照片挑选出来。如果有普通照片，就都扫描成电子版的，照片会丢失会退色，电子版的就不会咯。  

  2. 把所有照片放在一起，准备开始分类。  

  3. 接着在电脑里面新建一个目录，比如MyPhoto，然后在里面根据照片的类型建立新的目录，把同类的照片全部放入这些目录。不好分类的放入Other目录中。  

  4. 现在MyPhoto目录里只有一些目录，把一张无用的JPEG文件放在MyPhoto目录下。这个就是前面所说的技巧。因为如果MyPhoto里面只有目录，则每个目录会被单独列出，无法方便查看和管理。比较一下没有无用JPEG的效果：  ![](/images/2011/acidimage/skill1.gif)  ![](/images/2011/acidimage/skill2.gif)  

  5. 拷贝MyPhoto目录到/PALM/PROGRAMS目录下或者直接放在/下面。  

  6. 从某个我们都知道的网站下载AcidImage最新的xxx版。安装完毕。  

  7. 打开AcidImage，推荐设置如下：  
Load JPEGs Full Size：不选，让AcidImage自己缩放照片到适应整个屏幕的大小。  
Save Thumbs：always。有略缩图预览速度加快。  
SlidShow的Delay根据自己看幻灯片时的习惯设置秒数，这时间要减去打开图片所花的时间，我一般是设置为1秒。  
File Type只选JPEG就好了。  

  8. 平时一般都使用略缩图，列表模式少用。  

  9. 如果要观看，就使用SlideShow，快速查找使用Thumb。  

好了，电子相册做好了，可以自己欣赏或者拿出去炫耀了。。。  

