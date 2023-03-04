# 郑海山的个人主页

个人主页地址：[https://dog.xmu.edu.cn](https://dog.xmu.edu.cn)

微信公众号名“郑海山dump”，号为“zhsdump”。欢迎扫码关注互动。

![](https://dog.xmu.edu.cn/images/zhsdump.jpg)

## jekyll local setup

from https://jekyllrb.com/docs/installation/ubuntu/

```sh
sudo apt-get install ruby-full build-essential zlib1g-dev
sudo apt-get install libffi-dev libedit-dev libssl-dev
```

```sh
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

```sh
gem install jekyll bundler
bundle add webrick
```

## Preview

```sh
./startdebug.sh
```

http://127.0.0.1:4000

## Update

```sh
gem update
bundle update
```

## TODO

- /_posts/2020/2020-03-11-moodle-server-clustering-automatic-deployments.md mermaid
