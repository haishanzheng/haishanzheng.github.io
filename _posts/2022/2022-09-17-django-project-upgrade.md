---
title:  "某个Django系统升级记"
date:   2022-09-17 10:40:01 +0800
---

这篇是 [一个基于Python的Django项目正确的创建方式](/2022/08/11/django.html) 的一个后续。

## 整体架构升级

这个系统实际上包含了多个独立的站点，原先系统将几个功能部署在一起，这次重做，考虑到安全，高频低频，做了剥离。

内部管理的功能，部署在内网服务器上，并且不对外网开放，2个必须对外开放的。其中一个需要对外网提供服务，有**少量**动态的内容，为了安全和简化对外暴露服务器的配置，投机取巧使用了Apache的proxy模块，将需要动态的内容请求内网服务器来提供。数据用jsonp提供给外网服务器来调用。完美解决。

所以最终有4台服务器，2台对外，2台对内。

有一个功能需要去各个系统抓取数据，需要其他系统来信任这台服务器的IP地址。理论上也可以直接用内部管理的这台Web，但是内部管理功能也比较复杂，万一有个CVE漏洞，所以独立出来一台作为隔离。

总结下来就是：

A为管理端，B为支撑服务器，通过B向各个服务器发起请求。

X和Y为对外服务器。其中Y部署组件简化，请求代理给A处理。

通过这次架构梳理，整体系统更加安全和清晰了。

## IPv6检查对内外服务器带来的问题

某部门有持续在对IPv6支持度进行检查，检查会包括门户网站的二级链接、三级链接全部进行检查，导致一个比较尴尬的事情是，内网网站，如果没有IPv6域名，会导致被扣分，即使有，如果访问不到，也可能导致被扣分，神奇的设定。由于这次为了安全，将网站移到了校内，所以只能在各个位置删除这个网站的链接。

## MySQL utf8->utf8mb4

这是MySQL历史上的一个坑，很多人建立MySQL数据库会使用UTF8，但是在MySQL里面，为了兼容性，UTF8不是真正的UTF8，具体可看下面解释。这次升级顺带修复了。

> utf8 has been used by MySQL is an alias for the utf8mb3 character set, but this usage is being phased out; The utf8mb3 character set is deprecated and you should expect it to be removed in a future MySQL release. Please use utf8mb4 instead. utf8 is currently an alias for utf8mb3, but it is now deprecated as such, and utf8 is expected subsequently to become a reference to utf8mb4. utf8mb4 is a superset of utf8mb3.

升级的方法也比较暴力，直接mysqldump，

```sh
sed -i 's/utf8/utf8mb4/g' some_database.20220901.sql
```

然后导入新数据库，期待数据库内容里面本来就没有utf8这些文本吧！

## 断舍离

在整个项目的开发过程中，随着需求不停变更，或者一些临时性的需求带来的一些临时代码，项目最终会混乱到留下一堆乱78糟的代码。

但是因为懒，或者总是幻想未来可能用到。

干过乙方的都知道，甲方爸爸需求是不停变化的，变化才是永恒的，不变化那都是烂尾的项目。所以会通过一系列审批签字画押流程把变化变得困难来保持稳定。

**然而在这个项目里面我既是甲方也是乙方。。。**

躲不掉，这次借机全部把无用的代码全部梳理和删除，并且清理了所有的依赖和一些无用的数据库表。被删除的代码实际上在Git历史里还是可以找到的。

## 简化local_setting.py

本地配置里面有些敏感信息，是不进入Git的。原先在本地配置里面写了太多配置信息，实际上在测试环境和部署环境很多是一致的，所以尽量简化了local_setting.py。同时增加了检查，在Ansible自动化部署时，检查是否将template拷贝成真实文件，不存在就直接退出，使得Ansible运行起来更加完整安全。

## 组件全部升级到最新

项目里面用了一个Admin CSS框架，很久没有再活跃更新了，这次痛定思痛，切换到AdminLTE。CSS和JavaScript虽然CVE（DOM XSS）不像服务端那么严重，但是还是会被监管部门漏扫到，一并处理了。

## 数据验证使用H5

原先还使用jQuery Validation Plugin，但是由于H5自带了验证功能，所以前端就只使用H5来做个简单的验证，真正的验证逻辑在后端。

## Django的模板机制

用了Django和自带的模板引擎，导致一个问题是，你无法使用到前端技术发展带来的红利，比如React和Web Components为了前端可复用组件的封装功能。

比如说，我就是要前端展示一个卡片，在Django的模板里需要如下的内容：

```html
<div class="row">
    <div class="col">
        <div class="card">
            <div class="card-header">
                <h5 class="m-0">卡片标题</h5>
            </div>
            <div class="card-body">
                <p class="card-text">
                    卡片内容
                </p>
            </div>
        </div>
    </div>
</div>
```

为了代码比较好看，经过Format后，“卡片内容”会被缩进到很里面。最重要的代码前面20列都是空格，一定是不对的。如果用React不会有这个问题。

于是我思考了一个方法。

```html
{% raw %}
{% include widget_small_box with title="卡片标题" body="卡片内容" only %}
{% endraw %}
```

这里的raw是为了解决博客代码格式化带来的另外一个问题。

但是传入核心代码被放入字符串，很类似ASP、PHP没有出现之前的CGI模式，无法对字符串内部进行检查，无法语法高亮，而且逻辑和展示混合在一起，是错误的做法。

于是还是通过3个include来完成一个卡片的包装。代码如下：

```html
{% raw %}
{% include widget_card_title %}
卡片标题
{% include widget_card_body %}
卡片内容
{% include widget_card_end %}
{% endraw %}
```

通过以上方法，就将“卡片内容”缩进提升到最顶层，重复代码减少，如果未来要换个卡片样式，改一个地方就可以了。很聪明的做法！

## Python环境隔离问题

在前面一篇文章，我提到使用--user来隔离出类似ansible、ansible-lint的环境，但是我又学习到另外一个更好的方法，是使用pipx来安装所有命令行需要用到的组件。pipx可以做到，你要运行一个Python程序，他可以单独为这个程序打包好所有的环境，每个程序之间都是互相隔离的，而且比pip更方便升级。所有类似ansible、ansible-lint、djlint、flake8等等所有的只有IDE和命令行需要用到的，但是项目不需要用到的，都可以通过pipx来安装。

## Visual Studio Code的Litning和命令行的Linting

Visual Studio Code配置了完善的Format和Linting后，只会在你打开某个文件才生效。所以通过命令行运行诸如

```sh
djlint . -i "H006" --profile django
flake8 --ignore=E501,W503 --exclude 'node_modules'
ansible-lint
```

可以对全部项目代码进行检查，并且去修复所有的报警。（这个工作量非常大，完全是强迫症作祟）
