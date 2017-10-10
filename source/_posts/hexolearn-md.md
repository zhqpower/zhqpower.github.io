title: 搭建博客学习笔记
date: 2017-02-09 16:25:42
tags: hexo
---

分为两个分支，hexo 和master 分支，

  hexo 分支放原文件，也是工作分支。添加文章后，先提交git

  master 用于发布博客。 发布之前先rebase hexo 分支上的内容。


  1. 在工作分支(hexo)
     git add .
     git commit -m "will commit"
     git push origin hexo

  2. 在master分支
     git rebase hexo
     hexo g
     hexo deploy


  如果更换了电脑，git clone -b hexo  git@github.com:zhqpower/zhqpower.github.io.git

  把hexo 分支clone 下来，然后npm install 就可以了  



  ##### 常见操作
  创建文章: hexo new "我的第一篇文章"
