---
title:  "WebForm_PostBackOptions 未定义错误排查"
date:   2008-07-08 13:17:47 +0800
---

最近正好遇到2个都是WebForm_PostBackOptions 未定义的错误，2个不同的应用，引发了外表看起来一样的错误，但是最后解决了发现是不同的原因。

一个应用是从.net1.1升级到.net2.0，发现所有的linkbutton不能使用，均提示 WebForm_PostBackOptions 未定义 ，页面显示javascript错误，部分客户端可以正常使用，部分不能正常使用。最后发现是在Global.asax里面对输出作了修改，导致WebResource.axd被修改出错，于是添加过滤  
if (System.IO.Path.GetExtension(Request.Path).EndsWith(  
            "aspx", true, System.Globalization.CultureInfo.InvariantCulture)) {  
 return;  
}  
问题解决。

一个是sharepoint 2007，也是提示 WebForm_PostBackOptions 未定义 ，页面显示javascript错误，所有button，linkbutton均不能使用。所有客户端都不正常，在问题排查的过程中部分客户端可以使用部分不能使用。最后发现是由于系统时间错误，导致.net2.0在安装时系统时间为将来的时间，等知道后调回正确时间，但是请求Webresource.axd传入的时间为现在的时间，变成获取将来的资源，.net提示utcDate超出范围。于是重新更新.net2.0即可。

所以，总结了下，遇到 WebForm_PostBackOptions 未定义 ，webForm_PostBackOptions is undefined 等问题，要确认下是否Webresource.axd引起的错误。  
打开出错的页面，察看源代码，找到  

&lt;script src="/WebResource.axd?d=xxx&amp;t=xxx" type="text/javascript"&gt;&lt;/script&gt;

直接在IE里面输入这个地址看是否可以获取到文件，如果不行，检查IIS是否对axd做了映射，如果做了映射，是否去掉了“检查文件是否存在的”的限制。  
如果可以打开，察看文件大小，版本，仔细检查文件内容，跟从别人网站下载的有何区别。  
如果文件有错误打不开，要查看web.config是否设置了customError被重定向了。
