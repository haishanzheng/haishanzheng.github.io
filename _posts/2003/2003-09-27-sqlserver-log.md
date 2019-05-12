---
title:  "SqlServer Log 清除一法"
date:   2003-09-27 16:37:49 +0800
---

如果你在SqlServer里建立数据库忘了限制Log的大小，日久Log太大了，是不能直接在文件系统里面删除的。你可以试试这个办法。这个办法的缺陷就是，必须offline操作。这个办法也适用那种只有一个MDF文件恢复数据库的情景。  

  1. 首先备份数据库。假设数据库名为testlog，数据放在D:\SqlData下。  

  2. 使用下面命令deattach数据库，把数据库跟文件系统分开，这样在SqlServer的企业管理器里就看不到这个数据库。命令在Sql查询分析器里输入。  
exec sp_detach_db @dbname = N'testlog',   

  3. 接着改名log文件，在cmd.exe窗口内打入：  
改名咳咳 D:\SqlData\testlog_log.ldf D:\SqlData\testlog_log.NOUSE.ldf  

  4. 接着运行命令  
EXEC sp_attach_single_file_db @dbname = 'testlog',   
@physname = 'D:\SQLData\testlog_data.MDF'   
会提示  
设备激活错误。物理文件名 'D:\SQLData\testlog_log.ldf' 可能有误。  
已创建名为 'D:\SQLData\testlog_log.ldf' 的新日志文件。  

  5. 再次备份数据库。  

  6. 在数据库的属性窗口限制数据库LOG的大小。或者不限制，过几天再用这个办法清除。  
另一种办法是下载 [动网论坛Sql日志清除器 V1.1](http://www.mycodes.net/soft/235.htm) ，号称可以在线压缩清除sqlserver log。至今我还未使用成功过，如果你有使用经验，请跟我联系。  


## 评论

*****
**匿名** 在 *2004-12-02 10:35:41* 说: 好用，谢谢

*****
**匿名** 在 *2004-02-02 15:06:11* 说: 确是高手，一下就搞定了！
偶用了很多方法，下载了N多所谓日志清除利器都不行，还是你的办法好。

谢谢了！

