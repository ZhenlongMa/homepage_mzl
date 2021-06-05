---
title: failed to connect to github.com port 443的解决办法
summary: 
date: "2021-06-05T15:01:00+08:00"
publishDate: "2021-06-05T15:01:00+08:00"
lastmod: ""

view: 2

comments: true
profile: true
share: false

featured: false
headless: false
draft: false
private: false
---

在windows端使用git向github上提交代码时经常报错`failed to connect to github.com port 443`，有时候能顺利提交，但是大概率需要连续提交好几次才能成功。

使用git bash，输入`git config --global http.proxy http://127.0.0.1:7081`和`git config --global https.proxy http://127.0.0.1:7081`后解决。其中7081是我的翻墙软件绑定的端口。