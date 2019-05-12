---
title:  "编程技巧，重构"
date:   2004-04-26 22:10:46 +0800
---

最近在做一个php小项目，我发现我越来越受.net的影响了，在最大限度地分离业务逻辑层和表现层，做什么都在考虑分离、分离、分离。代码的美观和可维护性成了主要目的:(有点近乎过分了:)这里举几个非常小的例子，有问题欢迎探讨。  

你觉得这个  

{% highlight html %}
<form action=update.asp method=post><br />
<input type=hidden name=Action value=UPDATE><br />
<input type=text name=Nickname value=<%=Rs("Nickname")%>><br />
<input type=submit><br />
</form><br />
{% endhighlight %}

和这个  

{% highlight html %}
<form action=<%=Request.ServerVariables("SCRIPT_NAME")%> method=post name=form1><br />
<input type=hidden name=Action><br />
<input type=text name=Nickname><br />
<input type=submit><br />
</form><br />
<script language=javascript><br />
    TextSetValue(form1.Action, "UPDATE");<br />
    TextSetValue(form1.Nickname, "<%=Rs("Nickname")%>");<br />
</script>
{% endhighlight %}


有区别么？  

你觉得  

{% highlight php %}
Helper::ExecuteNonQuery("INSERT INTO tb_xx (xx1, xx2, xx3) VALUES ($xx1, '$xx2', '$xx3')");  
{% endhighlight %}

和  

{% highlight php %}
$sqlstr = "";  
$sqlstr .= "INSERT INTO tb_xx ";  
$sqlstr .= "(xx1, xx2, xx3) VALUES ";  
$sqlstr .= "($xx1, '$xx2', '$xx3')";  
Helper::ExecuteNonQuery($sqlstr);  
{% endhighlight %}

有区别么？  

你觉得代码写成这样有必要么？  

第一种，这样实现了“后期绑定”，这样我们可以重用add和update的form，action根据add.asp和update.asp自动变化，而且分离了显示层和数据层，是再次分离:)因为<%=Rs("Nickname")%>你可以用.net的Label，或者用Smarty的{$Nickname}，这是第一次分离。  
对于第二种，$sqlstr写成这样，可以更清楚得显示sql语句的层次，xx变量的对应关系也一目了然。而且给我们一个机会，如果需要调试，我们可以在$sqlstr被执行之前echo $sqlstr;exit;。  
对于第三种，删除和添加一个TextMustInput field非常方便，TextMustInput是自己写的Javascript。  

接下来有空将认真阅读《重构》，拿到这本书有点戏剧性，让我怀疑我同学ying是否私藏着水晶球。拿到这本书的前几天，我在图书馆上查找《重构》这本书（不知道什么原因想看。。。），发现已经被人借走了，其实我更希望一本书被人借走，在浩大的图书馆里查找一本书可不是一件简单的事，如果被人借走，则我就可以预约，等那个人还完书，收到预约通知，我就可以直接到图书馆总台去取:)我把他称为准外带服务。不过有人比我先预约了，总借数才个位数，为什么突然想看的人这么多？于是我也加入预订行列，指望前面的那人没看到图书馆异常简陋的预约通知，就这样等等等，等到那个可怜的人错过了预约时间，我再上去查看，发现居然没有通知我去取书，书已经被扔到基库了！！在这里强烈谴责深圳图书馆的图书管理系统。看来这个预订系统bug还真不少:)，没办法，第二天我就带着pda上面记载着这本书的索取号: TP311.11/826冲到了图书馆，满头大汗在里面找了半个小时，愣是没有找到（越来越希望图书馆开展送外卖服务，付费找书服务）。后来悻悻离去，在电梯门口遇到单位的小谌，告诉我我有份包裹，叫我跟他去办公室取，取来打开一看，里面赫然就有一本《重构》，汗。。。。咳咳。  

有什么感想，改年说:)  

