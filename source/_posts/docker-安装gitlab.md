---
title: docker 安装gitlab
date: 2017-09-23 22:32:38
tags: docker
---

##### docker 安装
https://github.com/moby/moby/releases

安装命令：curl -fsSL https://get.docker.com/ | sh

#### 安装compose
https://github.com/docker/compose/releases

curl -L https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose

#### 运行gitlab 镜像
http://docs.gitlab.com/omnibus/docker/#run-the-image

####通过编辑一个yml 文件来启动gitlab 镜像
 docker-compose.yml

 gitlab:
  image: 'gitlab/gitlab-ce:latest'
  hostname: '47.91.225.210'
  environment:
   GITLAB_OMNIBUS_CONFIG: |
     external_url 'http://example.com'
     gitlab_rails['gitlab_shell_ssh_port'] = 2222
  ports:
   - '2222:22'
   - '8080:80'
  volumes:
   - './gitlab/config:/etc/gitlab'
   - './gitlab/logs:/var/log/gitlab'
   - './gitlab/data:/var/opt/gitlab'
  restart: always


 启动命令： docker-compose up -d

 查看运行情况：
 docker-compose ps

docker-compose logs -f 查看日志
