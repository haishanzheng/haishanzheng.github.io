---
title:  "google和sourceforge2个小技巧"
date:   2003-06-09 15:29:16 +0800
---

对一个编程人员来说，google http://google.com 和sourceforge http://www.sourceforge.net 2个网站是最经常访问的，但由于某些知明原因，google的cache有时会使用不了，而sourceforge的域名一直无法正常解析。这跟使用的网络有关系，如果你正好碰到这个问题（我就是），那看看下面的2个小技巧。  

1. 关于google的cache：用shift点开google网页快照后，显示无法访问，这时在IE的地址栏里面在search?和q=cache中间添加任何一个变量，比如search?a=&q;=cache，这样cache就可以正常访问了。很显然，某个部门的url屏蔽系统只根据如果有search?q=cache就block这个url，我们在中间插入一个无用变量他就检测不出来了:)  

2. 我们在google上有时查到一个sourceforge的一个项目，而url是类似http://filez.sourceforge.net，这时点开访问不了，其实我们只需把这个url替换为http://sourceforge.net/projects/filez/就可以了。  

