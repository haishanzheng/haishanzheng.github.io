---
title:  "Bugzilla: A Bug's Life Cycle"
date:   2003-11-19 20:03:26 +0800
---

Bug管理系统管理的就是bug的状态，一个bug从出生到死亡的过程称为一个bug的[生命周期](http://bugzilla.mozilla.org/bug_status.html)。每个bug跟踪工具都有一套自己的规则，大多大同小异。Bugzilla用2个值来定义一个bug所处的状态，一个是Status，一个是Resolution，Resolution只有在Status为Solution后才有效。这是我画的Bugzilla的A Bug's Life Cycle。  
[![](/images/2011/tech/buglifelittle.gif)](/images/2011/tech/buglife.gif)  
一个Bug新加进来，首先是Unconfirmed，经过开发者的confirm，可以成为New或者Resolved。如果开发者认为这确实是一个Bug，并且需要修复，则把他设置为New。如果发现这只是用户的一个输入错误，或者知道这是一个Bug，但是不想修复，或者无法重现这个Bug，需要提供更多的信息，则可以设定为Resolved。接着在第二个Resolution设置Invalid，WontFix，WorksForMe。  

一个状态为New的Bug可以被指定给某个开发人员，等到该开发人员确认是他的任务后，状态成为Assigned。或者立刻被指定为Resolved。  

一个被Assigned的Bug，可能被重新指定给另外的开发人员。则状态重新变为New。或者被开发人员干掉，设置为Resolved，同时设定Resolution值，可能为Fixed或者其他。  

如果有设置Quality Assurance用户，则一个Resolved的Bug可以多一个步骤：等待QA的审核。审核通过，状态为Verified。  

被解决的Bug可以被设定为Closed。或者被重新打开。  

一个Bug如果死灰复燃，则进入Reopened状态，等待开发者的Accept。Reopened状态类似New状态。  


