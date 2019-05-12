---
title:  "源代码美化工具"
date:   2005-07-28 09:33:41 +0800
---

我写出的代码当然是不需要美化的，不过有时需要阅读别人的代码，或者别人写代码虽然很漂亮但是和你的格式不一样，就需要对代码进行美化修改。源代码美化工具只是对源代码重新格式化，不会对程序编译有任何影响。  

chedong的 C Java PHP Perl Python 的程序代码美化工具 (Pretty Print Program/Source Code Beautifier)使用 http://www.chedong.com/tech/indent_tools.html  

试了一下，下面这条命令对c#格式化效果和vs.net差不多。  
FOR /R %f IN (*.aspx.cs) DO astyle --mode=c --indent=tab=4 --indent-namespaces --brackets=break --break-blocks --pad=oper %f  

