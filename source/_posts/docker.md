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

      docker build -t + 生成的镜像名称:版本号 + dockerfile 文件路径

      docker build -t willvim:1.0 .

   docker bulid 有个上下文的概念，就是值当前文件夹。 build 时会把当前文件夹的所有文件发送给docker
   所以尽量将需要build 的文件放在一个文件夹下，也可以通过.dockerignore 文件配置忽略。

##### Dockerfile 文件编写

1. 所有的Dockerfile 都必须以from 作为第一条指令
2. Dockerfile 中指令建议大写
3. MAINTAINER 后面加作者名称，邮箱等信息。
    docker inspect + 镜像名称： 查看镜像详情
    docker inspect -f '{{.Author}}' 过滤某个字段
4. ENV + 变量名 + 变量值
         ENV PKG_NAME vim
         RUN apt-get install -y $PKG_NAME
5. RUN 指令
     第一种写法： RUN + 要执行的命令
     第二种写法： RUN ["apt-get","update"], 这种写法，不会处理环境变量
