---
layout: post
modal-id: 6
date: 2020-11-03
img: submarine.png
alt: image-alt
project-date: November 2020
client: Start Bootstrap
category: Dev Tips
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

**local test**

```sh
$ bundle exec jekyll serve
```

