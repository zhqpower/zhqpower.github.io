title: 搭建博客学习笔记
date: 2017-02-09 16:25:42
tags:
---

分为两个分支，hexo 和master 分支，

  hexo 分支放原文件，也是工作分支。添加文章后，先提交git

  master 用于发布博客。 发布之前先rebase hexo 分支上的内容。


  1. git add .
     git commit -m "will commit"
     git push origin hexo

  2. git rebase hexo
     hexo g
     hexo deploy
