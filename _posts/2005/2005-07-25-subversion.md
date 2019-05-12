---
title:  "Subversion源代码仓库规划一例"
date:   2005-07-25 10:12:46 +0800
---

Subversion的目录结构是很自由的，所有的规划都必须是你自己规定，考虑一个subversion仓库的目录树，你可以把任何一个目录认定为一个项目，你可以只checkout这个目录下的所有文件进行编码，跟CVS不同，CVS显式指定一个个module。所以你可以在一个仓库内保存多个项目，也可以一个仓库保存一个项目而使用多个仓库。我个人比较喜欢第二种，因为Subversion的每次commit都会导致整个仓库版本号增加一个，会使得多个项目的版本号出现断层。而且如果多个项目参与人不同，就必须使用apache2进行细粒度的权限控制，不是太方便。一个仓库一个项目，显得更优雅一些。  

以下是我研究出的仓库规划。  

在server端，新建一个目录用来存放所有的仓库。比如c:\svnrepos。然后在这个目录下建立每个项目独立的仓库。  
svnadmin create "c:\svnrepos\rolex"  
svnadmin create "c:\svnrepos\omega"  

使用 svnserve -d -r "c:\svnrepos" 启动。这样你的项目的url是：  
svn://IP/rolex  
svn://IP/omega  

在客户端新建一个目录，作为import的内容，比如c:\svnimport\rolex，然后在里面建立branches，tags，trunk子目录，把你需要源代码管理的项目放入trunk目录，注意删除垃圾文件。在c:\svnimport\rolex上点击Import...，选择url为svn://IP/rolex，导入。你可以使用仓库浏览器查看导入的效果。  

需要工作时，新建一个目录比如c:\svnclient\rolex\trunk，然后在trunk上checkout出svn://IP/rolex/trunk上的内容。  

