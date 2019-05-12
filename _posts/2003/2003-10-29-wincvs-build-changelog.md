---
title:  "wincvs 可自动 Build ChangeLog"
date:   2003-10-29 20:55:10 +0800
---

wincvs用了这么久，今天才发现居然可以自动Build ChangeLog。以前还在找是否有自动生成ChangeLog的cvs插件，看来，找到一个好的软件还不够，关键是要熟悉她。  

WinCVS->Macros->CVS->Build ChangeLog  

会自动在module下生成一个ChangeLog，格式不是太标准。想改改格式，可找不到可定制的地方。查看Macros菜单，发现一个Reload Macros，想想，是不是类似plusins的东西？就去wincvs安装目录查看，果然找到一个Macros目录，里面是各个macro，一般使用python语言写成。于是打算改改ChangeLog格式，把文件名和版本号去掉，只记录时间、作者和注释。python语言可不太熟悉，不过幸好天下语言是一家，通过grep大法，立刻就找到key code，注释掉cvs2cl.py  
# files.sort()  
# for file in files:  
# clFile.write( '\t* %s:\n'%file)  
这下好看多了，再在日期前加个  
***********************  
分割符，更pp了。  


## 评论

*****
**匿名** 在 *2005-03-02 21:26:52* 说: 请问我的wincvs的maros菜单怎么用不了啊?提示是:shell is not available!

