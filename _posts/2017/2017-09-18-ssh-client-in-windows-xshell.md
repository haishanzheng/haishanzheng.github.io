---
title:  "推荐中过木马的SSH客户端工具Xshell"
date:   2017-09-18 14:37:09 +0800
---

首先解释下标题：中不是种，过是指时间的曾经有过而不是说是种过的。我测试过很多个SSH客户端工具，由于安全运维和费用的问题，一直没有找到趁手的，直到某天在群里从北交大贺秋雨和审计大学吴鑫处得知Xshell有针对个人和学校的免费版。

但是Xshell被放过后门，XShell官方软件被植入后门溯源分析 http://www.freebuf.com/articles/terminal/144254.html ，从一个侧面说明这个软件开发商安全防护较弱，然而你是要信任一个从没暴露过安全问题还是一个曾经出现过安全问题的厂商，我选择了后者，我希望他们应该会有改进。

![](/images/2017/77-heartbreaks.jpg)

## 我的需求
- 不使用盗版，不使用任何修改原始EXE文件的盗版，各种破解可能被植入木马。
- 必须从原始站点下载。中文版putty后门事件分析 http://www.cnbeta.com/articles/tech/171116.htm 。
- 免费。办公经费买工具软件？我好像很少听说。（其实收费不高，如果被渗透损失比这个高多了，为什么不花钱买呢？公司为你买了一台i7、32G、512SSD，你说我还要正版Windows、Acronis、Office、Visio、JetBrains、Sublime Text 3、VS.NET、GitHub会员、Jira、PS、Acrobat、1Password，VMware Workstation、AWVS、IP Proxy Pool、国外SSH主机。。。滚）。
- 分组的地址簿：我要登录200台不同的服务器，如果无法分组是无法想象的。直接输入IP容易犯错，而且不同服务器私钥不同。

## 过往选择

首先可以看这篇2015年的文章 [10 best SSH Clients for Windows: free alternatives to PuTTY](https://www.htpcbeginner.com/best-ssh-clients-windows-putty-alternatives/)，里面列出的很多地址簿都无法分组，或者功能过于简单。PuTTY作为一个轻量级SSH客户端工具，只有一个EXE，功能齐全。唯一的问题就是地址簿无法分组，数据是写入注册表的。MobaXterm也挺好的，配色很漂亮，同时可以打开SFTP和SSH，只是不是免费的，免费Session只能10个，端口转发只能2个，宏4个。。。SecureCRT我用了很多年，默认的配色加粗蓝色前景色字体会跟黑色背景混淆，较难辨认，然后除了收费外，其他都很好。Terminals支持协议很多，但是我只用它来管理Windows远程连接，支持地址簿分组，分组打开，多标签，窗体大小自定义，Master Key。

## 所以我目前在使用的SSH客户端就是
- Xshell
- Bash on Ubuntu on Windows，Windows10创造者更新自带的Ubuntu服务器，用来首次添加公钥认证很方便，一条命令即可：ssh user@hostname 'mkdir -p .ssh && cat >> .ssh/authorized_keys' < id_rsa.pub
- PuTTY

## 地址簿转换
我通过Python写了个脚本，转换了我的200个地址簿和known_hosts。转换的逻辑很简单，把SecureCRT配置文件的Session Name，Hostname，Port，Username，Key（Identity Filename V2）取出，写入Xshell配置文件即可，都是.INI的格式。

## Xshell使用技巧
- 地址簿使用树形，地址簿不要有中文字符。建议以IP地址+机器名命名，比如“210.34.0.240 dog.xmu.edu.cn doghomepage”。
- 界面选择英文，否则QuickCommand被默认为“默认快速命令集”，而Session保存文件.xsh是ANSI编码的。
- Xshell配置文件没有SecureCRT的Global Options的概念。所以如果批量修改可能需要写程序。
- SFTP，xftp和lszrz分场景使用。SFTP为Xshell自带，xftp必须再下载软件。lszrz必须在系统上安装包。lszrz跟SFTP相比最大的好处在于，SFTP无法变换用户，而lszrz则没有这个限制，因为是通过ZMODEM协议传输。
- 善于利用Tab Tiled和Tab group调整多个Session布局。
- 可设置保留最后打开的Tab group，状态保持。
- 使用公钥认证，使用Xagent，无需频繁输入私钥密码。
- 启用Master Key，可放心保存系统密码，SecureCRT暂无这个功能。
- 使用端口转发突破防火墙，内网，本地服务。

## Xshell常用快捷键
- Alt+O，打开地址簿
- Alt+数字，切换Tab，配合Tab底色可以区分同一个主机的多个Session
- Alt+鼠标，块选择
- Ctrl+Ins拷贝或者设置copy on select，鼠标中键黏贴
- Alt+R透明
- Alt+S简洁模式
- Shift+方向键和Page Up Down查看Session log

