---
title:  "对PostNuke的RSS支持做的几个小修改"
date:   2003-07-09 22:35:44 +0800
---

1. 增加一个![](/images/2011/tech/xml.gif)图标，PostNuke的RSS链接太隐蔽，最好按业界标准？增加一个XML图片，链接到backend.php。  

以下2个为更改html目录下面的backend.php文件。  
2. 更改为GB2312编码  
&nbsp_place_holder;&nbsp_place_holder;&nbsp_place_holder;&nbsp_place_holder;&nbsp_place_holder;&nbsp_place_holder;&nbsp_place_holder;&nbsp_place_holder;echo "\n\n";  

3. 把0改为1，这样可以在FeedDemon内显示文章简介。  
&nbsp_place_holder;&nbsp_place_holder;&nbsp_place_holder;&nbsp_place_holder;&nbsp_place_holder;&nbsp_place_holder;&nbsp_place_holder;&nbsp_place_holder;$show_content = 1; // Decide if you want to include the hometext in the RSS feed (1=yes, 0=no)  

有兴趣，还可以把PostNuke缺省的RSS0.91版本改为Moveabletype的RSS1.0版本。改动也很简单，去查查RSS1.0的dtd就好了:)  


## 评论

*****
**agen bola tangkas** 在 *2015-03-11 19:20:56* 说: <strong>agen bola tangkas</strong>

对PostNuke的RSS支持做的几个小修改 | 郑海山的blog

