---
title:  "增加了全部评论的链接"
date:   2003-07-22 18:11:37 +0800
---

趁一点点时间修改了PostNuke的源代码，增加了全部评论的链接。现在可以点击全部评论查看所有其他网友的涂鸦，这样就可以集中查看新的评论了。过几天再弄一个查看最新10条评论的功能。大家有空多发表发表评论呀。。。  

本着修改最少的原则，做了以下修改，有需要请点击详细内容查看源代码级修改。  

html\modules\NS-Comments\index.php  

```php
#add for display all comments  
global $Haishion_Showallreply;  
if ("HAISHION_SHOWALLREPLY" == $Haishion_Showallreply) {  
    $q .= " FROM $pntable[comments] WHERE 1 = 1";  
} else {  
    $q .= " FROM $pntable[comments] WHERE $column[sid]=".pnVarPrepForStore($sid)." AND $column[pid]=".pnVarPrepForStore($pid)."";  
}  
#add for display all comments  

#add for display all comments  
if ("HAISHION_SHOWALLREPLY" != $Haishion_Showallreply) {  
    DisplayKids($tid, $mode, $order, $thold, $level);  
}  
#add for display all comments  

#add for display all comments  
case "showallreply":  
$info = pnVarCleanFromInput('info');  
$Haishion_Showallreply = "HAISHION_SHOWALLREPLY";  
DisplayTopic($info, $sid, $pid, $tid, $mode, $order, $thold);  
break;  
#add for display all comments  
```

附：  

comments改成只有单一的回复形式，反正comments不是很多哈。  

改变文件  
html\modules\NS-Comments\index.php  
597、608 pid一直=0

