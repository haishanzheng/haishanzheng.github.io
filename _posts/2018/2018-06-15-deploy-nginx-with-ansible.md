---
title:  "5分钟让你的老旧网站支持IPv6、HTTPS、HTTP/2，不能再多了"
date:   2018-06-15 15:28:09 +0800
---

领导让我一个月部署100台服务器，我刚花了一天时间写了个自动化脚本，我现在占着工位吹着空调喝着咖啡刷着抖音看Ansible帮我敲命令，我是不是很没有人性？我接下来29天只能这样玩了我该不该告诉领导？

注：Ansible不是某个同事的英文名，是一个自动化部署工具，同类型的还有Puppet、Chef、Salt等等。**有兴趣以后再介绍**。

## 本Git代码适用范围

假设你有一个老旧的Web站点 https://dog.xmu.edu.cn ，IP地址为IPv4 1.2.3.4 ，你再提供一台配置了IPv6的Ubuntu 18.04 LTS服务器，Clone我的代码，跑一条命令，会帮你把HTTPS和HTTP/2全部配置完毕，然后你测试正常后，修改下DNS，把 dog.xmu.edu.cn 指向新的IPv4和IPv6地址即可。

中间是无缝的，干净的，测试完备的。时间在5分钟。

## 具体步骤

- 安装一台Ubuntu 18.04 LTS，配置好IPv6地址。
- Clone代码 https://github.com/haishanzheng/nginx-install/tree/ansible ，最后会PR到  https://github.com/bg6cq/nginx-install 。
- cp hosts.template hosts.real，配置你的服务器IP地址、控制机IP、域名、上游原始IP等信息
- 跑一下 ansible-playbook site.yml -i hosts.real --ask-become-pass，5分钟安装完毕
- 跑一下 certbot --nginx certonly ，申请一个免费的Lets Encrypt证书。
- 再跑下 Ansible脚本，加入HTTPS支持。因为Ansible脚本是幂等的，所以你跑几千次都没问题。
- 跑下curl测试，强制域名指向新的IP地址和测试IPv6。curl --resolve dog.xmu.edu.cn:443:2001:da8:e800::42 -I --http2 https://dog.xmu.edu.cn -6 -v
- 如果curl正确，则更改DNS即可。再保险点，本地更改DNS，使用浏览器测试。
- 可以考虑提交网站到张焕杰的测试网站 https://ipv6.ustc.edu.cn/ ，肯定是100分以上。

## 代码

代码fork自中科大张焕杰的 https://github.com/bg6cq/nginx-install ，PR暂时未提交，张焕杰老师的文档对Nginx进行了加固，对系统进行了配置优化，可做为一步步操作手册，了解内部的具体配置机制，张焕杰也带了个sh自动化部署脚本，依赖较少。ansible目录为我编写的**无脑自动化部署脚本**，依赖Python3，可重复运行，幂等，会不定期同步张焕杰的配置。

Ansible跑起来类似这样子：

![](/images/2018/ansible.jpg)

其他不多说了，放假了，具体看README.md文档吧，如果真有人用，我考虑再更新，加个GoAccess统计啥的。。。
