---
title:  "向Internet开放Sharepoint网站的方法"
date:   2004-05-01 21:36:33 +0800
---

在开心就好的网站上看到一篇Microsoft SharePoint Portal Server做外网门户 http://blog.joycode.com/joy/posts/20389.aspx ，里面介绍了一个向Internet公布Sharepoint网站的方法，不过正好我以前也看到过相关的资料，于是下载比较了一下，各有优点，开心提供的方法（应该是刘润 http://www.run2me.com 的方法吧）比较巧妙，不过不算太标准，一个好处就是通过在非80端口映射，使得管理员无需输入用户名密码就可以管理。而Zandy Garrard介绍的方法则是利用了微软的Authentication Button http://msdn.microsoft.com/library/default.asp?url=/library/en-us/spptsdk/html/smpcauthenticationbutton.asp ，在iis开放Anonymous的同时又提供Integrated Windows Authentication，然后通过一个页面放置Authentication Button来通过管理员的登陆。看来是比较标准的做法:)看看msd2d.com的这篇文章：Setting Up Your Anonymous Portal http://msd2d.com/Content/Tip_viewitem_03.aspx?section=Sharepoint&id=a467007f-2633-4cf9-b4b6-b08b0d3cb485 。  

本单位的Sharepoint网站不打算向外公布:)虽然利用率不是太高，但是办公室要求每个人每天都必须打开Sharepoint，如果每次打开都必须输入用户名和密码，这势必影响大家的积极性。所以我采用了下面的方法：  
1。提供ChangePassword Webpart，让大家把密码修改得和自己的机器名密码一样。  
2。把单位网站加入到Intranet列表。在IE里。  
这样打开IE就直接以登录Windows的账号登陆Sharepoint，很方便。相关技术，参看How to prevent network login box when log on SPPS intranet http://msd2d.com/Content/Tip_viewitem_03.aspx?section=SharePoint&category=Administration&id=69cf96eb-9043-4a15-bba2-c19b4d0676c1

