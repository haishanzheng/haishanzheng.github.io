---
title:  "开始使用Subversion + TortoiseSVN"
date:   2005-07-24 14:29:41 +0800
---

Subversion http://subversion.tigris.org/ 对我最大的吸引力在于，他支持对目录进行版本控制，还有就是原子提交，这2个理由已经足够我抛弃CVS了，以前不用是怕他的技术不够成熟，现在我觉得使用的环境已经可以了，NAnt和ccnet都支持subversion。如果你有新的项目，你可以尝试使用Subversion代替CVS。如果想尝试的话，win下面的安装也很简单，10分钟就可以开始体验，下载了Subversion服务端和TortoiseSVN，服务端双击安装，到bin下面使用svnadmin create "c:\repos"，然后修改c:\repos\svnserve.conf，运行svnserve -d -r "c:\repos"就ok了。客户端安装后重起，就跟IE紧密结合在一起了，在任何一个目录右键都可以看到他。具体可查看Subversion或者TSVN的帮助文件。  

