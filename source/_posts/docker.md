---
title: docker
date: 2017-10-20 09:38:39
tags:
---

ps -ef  查看容器中的进程

saas
paas
laas
caas

soa
12-Factor App
基于容器技术向现代化转型
混合云，可移植性
Cloud Native


镜像是只读的，构建完成后不能修改。

容器是可写的。

docker images  | grep nginx 过滤镜像

docker inspect + 容器名称或者容器id


##### docker 镜像的构建

1. 启动一个容器，以交互的模式进入容器。在里面进行一些 安装软件等操作，最后通过docker commit 来生成一个新的镜像。

   docker commit + 容器id +新生成的镜像名称

docker commit 52b7f23a2e57 vim ==>生成一个名称为vim 的镜像

手动构建，繁琐，构建步骤容易遗忘。软件升级不方便。

2. 通过dockerfile 文件构建镜像
