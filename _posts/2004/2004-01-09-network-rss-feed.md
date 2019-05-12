---
title:  "网络中心RSS feed后续"
date:   2004-01-09 15:13:12 +0800
---

在前面的[网络中心新闻发布 Rss Feed]({{ site.baseurl }}/2003/12/05/network-rss-feed.html)里我曾经提到：奇怪的是在IE里面点击链接无法正常显示，在Editplus和FeedDemon下一切正常。  

我刚才无缘无故又想起来，就又查了一遍，格式是绝对正确的，奇怪的就是IE里无法正常显示。后来再查看PostNuke里的backend.php发现了一句  
header("Content-Type: text/xml");  
有点领悟，于是在asp里加入一句  
Response.AddHeader "Content-Type", "text/xml"  
果然就正常显示了。  

接着又修改了一下description，改成用，省得还得转换。。。呵呵。点击[这里](http://network.xmu.edu.cn/AnnRss.asp)查看。  

RSS是个好东西，应该在全校各职能部门网站提倡这个规范。  

