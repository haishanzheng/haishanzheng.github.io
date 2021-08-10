---
title:  "What is The Trackback"
date:   2003-11-09 19:17:58 +0800
---

什么是TrackBack，看看来自竹笋炒肉 http://hedong.3322.org/ 的1 http://hedong.3322.org/archives/000350.html 、2 http://hedong.3322.org/archives/000351.html 篇文章，我用比较通俗的语言来说明一下，如果一篇文章（通常表现为一个url）被你引用了，这只是一个单向链接，从你的文章可以到达被你引用的url，但是那个url不可能知道他被你引用了，为了实现双向的链接，使网络上所有的相关资源都网起来（这是共产主义社会），所以当你引用了某个url后，你就跟那个网站（通常是一个cgi或者php脚本）说一声，这个url被我的url xxx链接了，还有我的blog name是什么，等等信息，则被引用的网站会把你提交的这些信息保存起来，下次有人点击这个url的TrackBack就把他显示出来。原理是不是很简单？  

你会问：如果我实际上没有引用而随便去post我网站的url，会有什么事？  
当然没事了，看看我在hwTony Blog上刚做的测试 http://blogs.xmu.edu.cn/cgi-bin/mt-tb.cgi?__mode=view&entry_id=1241 ，一点事情都没有:)你不会被人砍更不会被要挟请吃hy什么的。  

很可惜，PostNuke没有提供自动TrackBack的功能，或许我没找到PlugIns。不过知道了TrackBack的技术实现，我们也可以手动做。  

1. 点击某篇文章的TrackBack链接，得到一个url。  
2. 用记事本拷贝TrackBack的url?url=你的url&title;=你的title&blog;_name=你的blog的名字&excerpt;=显示在对方那边的引用文字，然后替换内容，替换后可能为  
http://blogs.xmu.edu.cn/cgi-bin/mt-tb.cgi/230?url=https://dog.xmu.edu.cn&title;=测试tb&blog;_name=郑海山的Blog&excerpt;=显示在对方那边的引用文字  
3. 在IE里面提交这个URL，这样你的url就在对方的TrackBack数据库里面登记了。  
4. 你已经跟人家说了，所以现在你的文章里面可以引用他的url了，不过这个url不要跟TrackBack的url混淆起来咯。  


## 评论

*****
**匿名** 在 *2004-02-20 18:16:57* 说: 可怜的hwTony，那个URL被我们用来测试了一百遍、一百遍，呵呵

*****
**匿名** 在 *2003-11-09 21:43:42* 说: 　　你这个手工ping的方法，很有创新啊。我一直在想，如果没trackback的功能的系统，怎么来ping啊？有你这个，问题似乎容易了。hehe.

　　不过有一点，就是trackback规范已经声明，初步放弃对get方法的支持，此时用这就方法可能会出一些问题。

Hilton


