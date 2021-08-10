---
title:  "SharePoint HRESULT 中的异常：0x80040E14"
date:   2004-09-13 15:54:20 +0800
---

前天访问单位内部的sharepoint，访问首页显示500错误，访问其他页面显示 HRESULT 中的异常：0x80040E14 ，查找事件查看器，也没发现什么不妥，出了一身冷汗，这个sharepoint，使用一个web.config把所有的web请求都handle了，然后所有的东西都放在数据库内，出了一点点小问题还真不知道如何下手解决。  

今天试着google sharepoint HRESULT 0x80040E14，立刻就找到了KB841216 http://support.microsoft.com/default.aspx?scid=kb;EN-US;841216&FR=1 ，问题立刻解决。  

When you connect to your Microsoft Windows SharePoint Services Web site after you install a Microsoft Windows SharePoint Service service pack on the server, you may receive an error message that is similar to one of the following:

Exception from HRESULT: 0x80040E14.
Troubleshoot issues with Windows SharePoint Services.  

HTTP 500 - Internal server error

咳咳，能解决就好。  

