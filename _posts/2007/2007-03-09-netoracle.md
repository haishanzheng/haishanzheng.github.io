---
title:  ".net访问oracle数据库最简便的方法"
date:   2007-03-09 21:09:45 +0800
---

这里提供一个方法，无需安装oracle client，真正的绿色访问方式。  

下载oracle10g开始才有提供的 instantclient http://www.oracle.com/technology/tech/oci/instantclient/index.html。  
解压，里面就几个dll和jar，把解压的路径放入path变量，给你的.net程序添加System.Data.OracleClient引用，这样你就可以使用类似  

{% highlight csharp %}
    return @"  
    Data Source=(DESCRIPTION=  
    (ADDRESS_LIST=(ADDRESS=(PROTOCOL=TCP)(HOST=xxxxxxxxx)(PORT=1521)))  
    (CONNECT_DATA=(SERVER=DEDICATED)(SERVICE_NAME=xxxxxxxxx)));  
    User Id=xxxxxxxx;  
    Password=xxxxxx;  
";  
{% endhighlight %}

的连接字符串访问oracle数据库。  

如果你还是喜欢用类似  

{% highlight csharp %}
    Data Source=xxxx;  
    User Id=xxxxxxxx;  
    Password=xxxxxx;  
{% endhighlight %}

的连接字符串，那就要多做下面几个步骤：  

在instantclient目录下建立 network\admin，然后放入 tnsnames.ora 和 sqlnet.ora (你应该知道里面填什么内容吧)，然后设置2个环境变量  
TNS_ADMIN -> network\admin   
ORACLE_HOME -> instantclient  

可以了。  

如果你的应用比较关键，还是推荐老老实实下载Oracle Client安装并使用Oracle Data Provider for .NET (ODP.NET)。  

