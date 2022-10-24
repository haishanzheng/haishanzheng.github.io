# 郑海山的个人主页

个人主页地址：[https://dog.xmu.edu.cn](https://dog.xmu.edu.cn)

微信公众号名“郑海山dump”，号为“zhsdump”。欢迎扫码关注互动。

![](https://dog.xmu.edu.cn/images/zhsdump.jpg)

## jekyll local setup

    vagrant up

    sudo apt install ruby

    sudo apt install ruby-dev
    sudo apt install build-essential
    sudo apt install zlib1g-dev

    sudo gem install bundler
    bundle install

## Preview

    cd /vagrant/
    ./startdebug.sh
    bundle exec jekyll serve --incremental --watch --force_polling

http://127.0.0.1:6940

## Update

    sudo gem update
    bundle update

## TODO

- /_posts/2020/2020-03-11-moodle-server-clustering-automatic-deployments.md mermaid
