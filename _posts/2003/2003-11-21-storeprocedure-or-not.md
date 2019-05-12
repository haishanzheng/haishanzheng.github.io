---
title:  "存储过程 or not"
date:   2003-11-21 20:43:33 +0800
---

我最近在看一些asp.net的源代码，这些源代码大部分来自于[asp.net](http://www.asp.net)，我发现有几个源代码里把所有的sql语句都写入存储过程，接着使用一个Sql类访问存储过程。这是我所未用过的设计，曾经听过Beta说起厦门某烟草公司用的就是这种设计，客户端瘦得没有任何sql语句。对于这种极端的手法我是不太赞成的，我写程序最先考虑的是可维护性，存储过程我比较少写，这样的用处很明显，把数据层都放在了一起，但是我想象可维护性肯定非常低。我至少可以提出2点。  

1.如何维护这么一大堆存储过程脚本？有什么好的存储过程编写和调试工具？每个存储过程更新都得先drop再create。这比我直接在代码里面改sql语句并立刻生效可麻烦多了。  

2.原先我们在程序都是写动态的sql语句，如果某个地方出错，我们可以在sql语句真正执行之前把sql语句打印出来并退出程序。而如果使用了存储过程，调试时我们得在2个调试器之间来回切换，得手动构造存储过程调试数据。  

我有点困惑，如何在这之间取得平衡，于是上鼓浪听涛询问，在SoftEnginee版和webapp版都发了同样的文章，可惜反响不是太强烈，afan给了很多宝贵的意见，还有kanhaichw，（虽然有些是错的：（），ganr，但是我的困惑还是没怎么解除。  

直到偶然在Rob Howard's Blog上看到了一大帮人关于存储过程的讨论，Frans Bouma是一个不使用存储过程的极端_I used them a lot for 8 years, but I'm now almost stored procedure 'free' for 8 months now, and I love the feeling. _，Rob Howard却是_my personal preference will be to use stored procedures for the majority of data access code_，高手过招，这么多文章让我激动得喘不过气来。你也来看看吧。  

事情的起因：[Don't use stored procedures yet? Must be suffering from NIHS (Not Invented Here Syndrome) ](http://weblogs.asp.net/rhoward/posts/38095.aspx)  

来自Frans Bouma的反驳：[Stored procedures are bad, m'kay? ](http://weblogs.asp.net/fbouma/posts/38178.aspx)  

Rob Howard 反击：[Continued](http://weblogs.asp.net/rhoward/posts/38446.aspx)  

其他人：[Why stored procudures can be evil? ](http://mehranikoo.net/mysite/posts/212.aspx)  
[Stored Procedures versus Dynamic SQL - the old debate...Frans Bouma's take...](http://www.mostlylucid.co.uk/posts/622.aspx)  
[Stored procedures vs Dynamic SQL](http://objectsharp.com/blogs/bruce/posts/201.aspx)  
[Stored Procedures or not? ](http://todotnet.com/posts/197.aspx)  
。。。  

以前链接全部来自blog，我爱blog。。。  


## 评论

*****
**匿名** 在 *2005-02-01 16:19:00* 说: 存储过程只是把logic细化，用存储过程要考虑绑定的问题。

*****
**郑海山** 在 *2003-12-09 11:18:30* 说: 回percyboy，我不知道oracle，db2如何，但sql2k对组合sql语句有做了一些处理，所以组合sql语句跟存储过程效率可能差不了多少了。不过有一条组合sql是不能替代的，就是存储过程可以返回尽量有用的值。

安全性，组合sql用Parameters也可以做到很好的安全性。下面这段代码既美观又安全。

        public void AddGuestbook(string title, string content) {
            string sqlstr = "INSERT INTO tb_guestbook (Title, Content) VALUES (@title, @content)";

            OleDbConnection myConn = new OleDbConnection(GetConnString());
            myConn.Open();

            OleDbCommand myCmd = new OleDbCommand(sqlstr, myConn);
            myCmd.Parameters.Add(";@title", OleDbType.VarChar, 255).Value = title;
            myCmd.Parameters.Add(";@content", OleDbType.VarChar, 255).Value = content;
            myCmd.ExecuteNonQuery();

            myConn.Close();
        }

关于安全性，可以参考这篇文章，http://www.microsoft.com/china/msdn/library/SecurityGuide/Chapter12.doc

*****
**郑海山** 在 *2003-12-09 11:07:24* 说: 谢谢。。。发现时间慢了一个小时。。。已经调整了呵呵。。。

*****
**匿名** 在 *2003-12-08 23:33:58* 说: 突然发现你的服务器用的时区是东七区，比北京时间晚一个小时。呵呵，服务器在哪里啊？
加个参数校正一下贝......

*****
**匿名** 在 *2003-12-08 23:31:24* 说: 很显然，存储过程的最大好处是运行效率还有安全性。

由于存储过程是编译执行，而且经常用到的存储过程会被高速缓存，这些都大大提高了运行效率；相反的，通过代码组合 SQL 语句，除了众所周知的安全性问题（就是被人恶意写入特殊文本）外，还需要即时编译，即时运行，并且无法缓存，这些都会影响其效率，特别是在访问高峰时有可能会形成瓶颈，影响响应速度和吞吐量。

percyboy AT 800e DOT net


*****
**郑海山** 在 *2003-11-22 22:20:03* 说: 我觉得不用sp也可以达到很好的分层。分层和sp之间没有必然联系。我以后的做法将会是sp和嵌入式sql语句并行，对于需要返回分页信息的，就用sp写，这样可以加快速度，并减少数据库和应用程序的交互流量。对于简单的insert和delete，用嵌入式sql，如果一个业务逻辑需要多个数据库操作，也用sp写。等等。。。

而嵌入式sql采用参数的写法，这样可以加快速度和提高安全性。不过以前构造那些var SqlStr的技巧就都要作古了。。。

*****
**匿名** 在 *2003-11-22 20:47:18* 说: <p>大量使用存储过程有非常多的好处，至少快速原型开发时如此。我经历过的一个MIS系统实现之初，我和客户都不知道会是什么样子。原型出来之后，我们没有把原型拆掉，而是使原型的界面使用固定的视图。之后开发界面的人和从来没有改动过数据引用，一切业务逻辑都是在数据库实现的，有一些存储过程写出来有两三页长。项目进行得还可以。</p><p>如果有两三个人开发程序，分离SQL和程序代码的好处也许不显然，但是如果需要<strong>多人</strong>（>5）合作便不同。多人合作时难以互相告知最新的代码更动（和后面的思想），这样产生难度，也就是brooks所说的“平方复杂度”。分离业务逻辑很有好处。</p><p>我喜欢“尽可能的分层”，现在感到有时候这也会成为累赘。有一段时间我喜欢大量使用样式表和HTML表现（htc），结果我和我的网页设计合作者都感到很头痛。我以为减少了他的难度，实际上增加了他的麻烦。大量使用存储过程很容易变得“实际上增加了麻烦”。此外还有移植的困难，要紧靠ANSI99才好。过些时日待MySQL实现了ANSI99，程序也可以跑在MySQL上，这样便兼容性很好了。</p>

