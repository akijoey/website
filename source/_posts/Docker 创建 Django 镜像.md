---
title: Docker 创建 Django 镜像
date: 2020-01-19
tags: Docker
categories: Backend
keywords: Docker
description: Docker
cover: /img/cover_posts/docker.jpg
top_img: /img/cover_posts/docker.jpg
---
## 创建容器

原始镜像为 `akijoey/wsgi`.

### 获取原始镜像

`$ docker pull akijoey/wsgi`

### 创建容器

`$ docker run -d -p 8000:80 --privileged --name django akijoey/wsgi /usr/sbin/init`

`--privileged` 启动 dbus-daemon
`/usr/sbin/init` 设置 CMD

### 进入容器

`$ docker exec -it django /bin/bash`

## 安装相关环境

安装 git, django 及相关扩展.

### 更新 yum

`$ yum -y update`

### 安装 git

`$ yum -y install git`

在 `/var/www` 下获取项目.

### 安装 django

`$ pip install django`

## 修改相关配置

修改 nginx, uwsgi 相关配置.

### 修改 nginx 配置

修改 `/etc/nginx/nginx.conf` 配置如下:
```conf
server {
	location /static {
		alias /var/www/django/static;
	}
}
```

### 修改 uwsgi 配置

修改 `/etc/uwsgi.ini` 配置如下:
```ini
[uwsgi]
master = true
vacuum = true
socket = 127.0.0.1:9000
pidfile = /var/run/uwsgi.pid
daemonize = /var/log/uwsgi.log
wsgi-file = django/wsgi.py
chdir = /var/www/django
```

## 测试及打包镜像

测试服务及打包镜像.

### 新建 wsgi.py

将 `/var/www/wsgi/index.py` 移动到 `/var/www/django/django/wsgi.py`

`$ mv /var/www/wsgi/index.py /var/www/django/django/wsgi.py`

### 打包镜像

退出容器.

`$ exit`

将容器打包成镜像.

`$ docker commit django akijoey/django`

>镜像位于 Docker: https://hub.docker.com/r/akijoey/django