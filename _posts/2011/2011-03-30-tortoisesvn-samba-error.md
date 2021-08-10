---
title:  "TortoiseSVN在samba环境下提交代码出错的解决方法"
date:   2011-03-30 15:57:16 +0800
---

TortoiseSVN是一个非常好用的svn GUI客户端，利用TortoiseSVN检查本地更新（svn st），diff，svn log，browser repository非常方便。所以我一般开发LAMP程序也会在把站点目录使用samba映射出来，然后在windows环境下使用TortoiseSVN。通过网上邻居访问svn会使得commit和svn st速度变慢，特别是有大量文件时，但是跟使用的方便性而已，这点速度流逝还是可以忍受的。

&nbsp;

然而在某个条件下，还有可能导致另外一个更严重的错误。每次commit后会提示无法移动某些文件出错，特别是有新add文件上去的时候。通过研究这些文件。
<p lang="en-US">ls -lah</p>
-r-xr-x--- 1 www-data www-data 192 2011-03-30 09:58 all-wcprops

发现在.svn目录里面的svn控制文件，权限会变成只读，导致了无法对这些控制文件移动或者删除，使得svn无法正常工作。这时可以删除出错的目录，并且使用TortoiseSVN进行clean up。但是每次commit都会不定期出现这个错误。

&nbsp;

按理说TortoiseSVN新建立的文件是受samba的create mask控制的，通过测试umask和create mask，一切正常，TortoiseSVN其他建立的文件和目录也都正常受create mask控制，唯独.svn里面的几个文件w权限全部被删除。经过查询得知原来是samba的一个bug。

&nbsp;

Permission problems with working copies on a SAMBA share.

After upgrading to TortoiseSVN 1.5.x or later, you get a lot of "Access denied" errors for most of the Subversion commands if your working copy is stored on a SAMBA share.

Some users reported that the problem went away after they upgraded SAMBA to the latest version. If that does not help or you can't upgrade, allow readonly files to be deleted in the SAMBA config file:

[global]
http://us1.samba.org/samba/docs/man/manpages-3/smb.conf.5.html#DELETEREADONLY delete readonly= yes

For older versions, try:

[global]
create mask = 0644
http://us1.samba.org/samba/docs/man/manpages-3/smb.conf.5.html#FORCECREATEMODE force create mode = 0600
security mask = 0555
force security mode = 0600

The information we have received suggests that the main problem is fixed in SAMBA 3.2.3. There is a supplementary problem with making files with the svn:needs-lock property read-only. This is reported to be fixed in SAMBA 3.2.6 or 3.3.0.

&nbsp;

源文档 &lt;http://tortoisesvn.tigris.org/faq.html http://tortoisesvn.tigris.org/faq.html &gt;

&nbsp;

加了force create mode后一切正常。

&nbsp;

又：TortoiseSVN在网上邻居默认不显示图标，可以修改Setting-&gt;Icon Overlays-&gt;Drive Types-&gt;Network drivers。

&nbsp;

&nbsp;

## 评论

*****
**王偌琳** 在 *2012-01-12 17:39:21* 说: 受教了解决我问题了谢谢了~~！

