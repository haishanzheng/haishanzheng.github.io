# 郑海山的个人主页

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
