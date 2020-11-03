---
layout: post
modal-id: 6
date: 2020-11-03
author: nandy
img: submarine.png
alt: image-alt
project-date: November 2020
client: Start Bootstrap
category: dev-tips
title:  "[DevTips] How to make github blog"
description: How to make github blog by nandy
---
# How to make blog

Address : [dongjinnam.github.io](https://dongjinnam.github.io)

Referenced From : [Link](https://zoomkoding.github.io/gitblog/2019/08/15/git-blog-1.html)

Theme : [Centrarium](http://jekyllthemes.org/themes/centrarium/)

Ruby Version : **2.6.6** (`2.7.0 이상`은 에러 납니다)

**Tutorial**

```sh
$ wget https://github.com/bencentra/centrarium/archive/master.zip
$ unzip centrarium-master.zip
$ cp ./centrarium-master/* ./dongjinnam.github.io/
$ cd ./dongjinnam.github.io
$ gem install bundler
$ bundle install
```

**Update _config.yml**

```yml
...
url: "dongjinnam.github.io"
...
```

**Update**

```sh
$ cd ./dongjinnam.github.io
$ git add .
$ git commit -m "[Update] Centrarium theme _config.yml"
$ git push origin master
```

**Local Test**

* 로컬 테스트 시, ruby 가 global 환경으로 설치가 되어야 합니다.

**Ubuntu 환경 ruby 설치**

```sh
$ sudo apt update
$ sudo apt install autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm5 libgdbm-dev
$ git clone https://github.com/rbenv/rbenv.git ~/.rbenv
$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
$ echo 'eval "$(rbenv init -)"' >> ~/.bashrc
$ source ~/.bashrc
# ruby 전역 2.6.0 이상 설정
$ rbenv install 2.6.0
# ruby version check
$ ruby -v 
```

**로컬 서버 기동**

```sh
$ bundle exec jekyll serve
```

