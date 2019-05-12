---
title:  "install on demand"
date:   2004-11-22 23:51:40 +0800
---

在最近做的一个小程序中，需要lamp + perl，程序很简单，配置环境倒是很麻烦，幸好这几个系统都有自动安装的工具。  

首先是linux，使用yum，安装一个包只需yum install packagename即可，所以就首先安装了一个最简单的系统，然后安装apache，mysql，php等等，遇到无法编译无法通过的，根据错误信息猜或者把错误信息放入google一下就可以找出包名，然后yun install，很是方便。  

其次是perl，大名鼎鼎的cpan写perl的都知道吧，要使用某个module，只需先perl -MCPAN -e shell，然后install modulename，自动download module，configure，make，make test，make install。  

然后是php，php的pear就是模仿cpan形式的类库，只要上pear站点去查找你需要的某个类库，然后pear install xxx就可以使用了。  

（色迷迷）我喜欢。。。给他们取个名字，叫install on demand哈。  


## 评论

*****
**匿名** 在 *2004-11-28 19:26:03* 说: 为什么不试试apt呵呵

