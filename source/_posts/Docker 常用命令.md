---
title: Docker 常用命令
date: 2019-11-09
tags: Docker
categories: Backend
keywords: Docker
description: Docker
cover: /img/cover_posts/docker.jpg
top_img: /img/cover_posts/docker.jpg
---
## 查看版本

`$ docker --version`

## 查看本地镜像

`$ docker images`

-a 查看本地所有镜像 (包括中间层镜像).

## 查看容器

`$ docker ps`

-a 查看所有容器 (包括未运行的容器).

## 创建新容器

`$ docker run -d -p 8080:80 --name CONTAINER IMAGE`

`-d` 后台运行容器.
`-p` 指定端口映射.
`--name` 命名新容器.
`--privileged` 启动 dbus-daemon
`/usr/sbin/init` 设置 CMD

CONTAINER 为容器名.
IMAGE 为镜像名.

## 启动未运行的容器

`$ docker start CONTAINER`

## 停止运行中的容器

`$ docker stop CONTAINER`

## 重启容器

`$ docker restart CONTAINER`

## 删除容器

`$ docker rm -v CONTAINER`

`-v` 删除数据卷.

## 删除容器

`$ docker rmi IMAGE`

## 在运行的容器中执行命令

`$ docker exec -it CONTAINER /bin/bash`

`-i` 交互模式运行容器.
`-t` 分配虚拟终端.

## 退出容器

`$ exit`

## 将容器打包成镜像

`$ docker commit CONTAINER USERNAME/IMAGE`

USERNAME 为 Docker Hub 用户名.

## 创建镜像

`$ docker build -t USERNAME/IMAGE .`

使用当前目录的 Dockerfile 创建镜像.

## 登录 DockerHub

`$ docker login`

## 登出 DockerHub

`$ docker logout`

## 从远程仓库拉取镜像

`$ docker pull IMAGE`

## 将镜像推送到远程仓库

`$ docker push IMAGE`

## Centos6.x 安装 docker

`yum -y install https://get.docker.com/rpm/1.7.1/centos-6/RPMS/x86_64/docker-engine-1.7.1-1.el6.x86_64.rpm`