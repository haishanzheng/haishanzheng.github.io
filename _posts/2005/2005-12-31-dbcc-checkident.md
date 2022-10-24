---
title:  "DBCC CHECKIDENT 改进"
date:   2005-12-31 00:33:47 +0800
---

DBCC CHECKIDENT可以用来重新指定表的主关键列的标识值，这个在你的标识值耗尽（不太可能），或者删除所有数据后需要回收标识值时非常有用。不过他有一个很bt的特性：  

DBCC CHECKIDENT ('table_name', RESEED, new_reseed_value)  

The current identity value is set to the new_reseed_value. If no rows have been inserted to the table since it was created, the first row inserted after executing DBCC CHECKIDENT will use new_reseed_value as the identity. Otherwise, the next row inserted will use new_reseed_value + 1. If the value of new_reseed_value is less than the maximum value in the identity column, error message 2627 will be generated on subsequent references to the table.  

也就是说，如果user表是刚建立的，我reseed为1，则插入数据后的ID是1，如果原先已经有数据（可能被删除），reseed为1后，插入数据后的ID为2，我不知道MS为什么要这么设计。  

我们做测试时有时是这样，启动一个事务，做一些初始设置，比如清空所有数据，然后开始测试，测试完毕会滚事务。在测试时为了方便，我们会假定插入一个用户后他的ID为1，这样我们才能获取并比较，由于有了上面这个特性，我们假定就会失败。  

不过失败只发生在第一次测试运行时，以后就正常了。  

TRUNCATE TABLE也可以清除表内所有数据，速度比DELETE FROM更快，而且自动reseed，但是一个最大的问题是，如果你的表有Foreign Key约束，则无法执行这个操作，即使你的子表没有任何数据。所以基本是废的。  

当然，要解决上面的问题很简单，你不管3721只需在DELETE之前先INSERT一些dummy数据，然后再reseed为0，就正常了。不过这样的解决方案很土。有2个地方土，一个是表有些字段是NOT NULL的，所以INSERT都要把这些字段写进去，如果表字段改名或者新加入NOT NULL的字段，你的INSERT语句必须更新。这在开发时很正常。第二个土在于，如果你一个表Foreign Key另外一个表，你INSERT之前必须知道另外表的ID。  

我在网上查了很久都没有人有解决方法，后来在做别的事情时突然想到Sqlserver应该会把这个信息保存在某个地方，通过查找系统表，果然找到了记录的位置。一个表如果自创建后没有插入过值，则last_value为NULL，否则为某个数字。  

以下解决方法只在sqk2k5有效，2k请自行查找系统表位置。  

```sql
IF EXISTS (SELECT * FROM sys.identity_columns WHERE last_value IS NULL AND name = 'HaishionId')  
DBCC CHECKIDENT (tb_Haishion, RESEED, 1)  
ELSE  
DBCC CHECKIDENT (tb_Haishion, RESEED, 0)
GO  
```

上面的代码只依赖表的主关键字段，这样子reseed后你插入后得到的ID一直为1。  

