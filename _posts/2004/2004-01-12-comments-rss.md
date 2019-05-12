---
title:  "增加Comments RSS"
date:   2004-01-12 17:54:59 +0800
---

模仿backend.php，写了一个Comments RSS的文件，这样我就可以时时关注本站的评论了:)有什么问题尽管提咯。  

以下解决方案，给有使用Postnuke的人参考。  
rename backend.php到另外一个xxx.php文件，修改  

$title = pnVarPrepForDisplay(pnConfigGetVar('sitename'));  
$title .= " 网友评论";  

$sql = "SELECT pn_subject, pn_comment, pn_tid, pn_sid FROM $pntable[comments] ORDER BY pn_date DESC";  

然后title从$subject加 ($tid)读取，后面要加tid，否则FeedDemon会认为是同一篇文章。或者你可以用RSS2.0的格式写，不过这个改动就大了:(  

$link可以修改成点击回复Comment的链接。  

$content就是$comment了。  


## 评论

*****
**郑海山** 在 *2004-01-16 14:56:00* 说: 新发现，$title不用加( $tid )，这样显示起来很不好看，把$tid加到链接后。

$link = xxxx &tid=$tid



