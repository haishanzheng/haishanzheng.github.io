---
title:  "修改AD登录密码的WebPart"
date:   2004-04-12 22:42:55 +0800
---

我不知道SharePoint是否已经提供了修改密码的功能，自己写了一个WebPart修改Active Directory内的账号密码。核心代码是：  

    string userName = SPControl.GetContextWeb(Context).CurrentUser.LoginName;  
    userName = userName.Remove(0, userName.LastIndexOf('\\') + 1);  

    DirectoryEntry user = new DirectoryEntry("WinNT://./" + userName);  
    try
    {  
        user.Invoke("ChangePassword", new object[]{oldPsw, newPsw});  

    }

    catch (Exception ex)
    {  
        output.WriteLine("出错啦");  
    };  

其他链接：  
SPS sdk http://www.microsoft.com/downloads/details.aspx?FamilyID=1c64af62-c2e9-4ca3-a2a0-7d4319980011&DisplayLang=en ，只包含一个文件MicrosoftWindowsSharePointServicesSDK.chm。  

A Developer's Introduction to Web Parts http://msdn.microsoft.com/library/default.asp?url=/library/en-us/odc_sp2003_ta/html/sharepoint_northwindwebparts.asp ，教你一步步编写WebPart。  


## 评论

*****
**匿名** 在 *2005-03-11 17:10:22* 说: 能否有编译过的例子让我们看看呢？
多谢了。
我的电子邮件jianghanlong@hotmail.com

*****
**匿名** 在 *2005-01-20 11:52:03* 说: 不知道为什么，用这段代码总是会抱错啊，webpart加载会失败，去掉这段代码，一切正常。能不能把你的webpart的全部代码发给我一份呢？我的email: pzcai@21cn.com.  非常感谢！！（寻求sps修改密码的webpart很久，但是最后还是不得不自己来学着写webpart，总算学会了基本的界面和安全发布等，但是调用修改密码这部分还是不行啊，苦求中…………）

