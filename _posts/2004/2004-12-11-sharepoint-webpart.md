---
title:  "Sharepoint webpart开发"
date:   2004-12-11 12:03:57 +0800
---

说说我的webpart开发步骤吧：  

下载 Microsoft Windows SharePoint Services SDK  
安装WebPart Templates for VS.net  
新建一个WebPart Library项目  
新建一个Cab项目，把第一个项目的输出等加入  
使用stsadm安装开发的WebPart  
安装完wwwroot的bin下面会有你的webpart的dll，所以。。。把第一个项目的输出路径改到wwwroot去  
开始漫长的调试生涯吧。。。。  

相关工具：  
InstallAssemblies  
SmartPart  


