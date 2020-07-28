---
title: Docker 创建 WSGI 镜像
date: 2020-01-19
tags: Docker
categories: Backend
keywords: Docker
description: Docker
cover: /img/cover_posts/docker.jpg
top_img: /img/cover_posts/docker.jpg
---
## 创建容器

原始镜像为 `centos:8`.

### 获取原始镜像

`$ docker pull centos`

### 创建容器

`$ docker run -d -p 8080:80 --privileged --name wsgi centos /usr/sbin/init`

`--privileged` 启动 dbus-daemon
`/usr/sbin/init` 设置 CMD

### 进入容器

`$ docker exec -it wsgi /bin/bash`

## 安装 WSGI

安装 nginx, mysql, python 及相关模块.

### 更新 yum

`$ yum -y update`

### 安装 nginx

`$ yum -y install nginx`

### 安装 mysql

`$ yum -y install mysql-server`

### 安装 python 及相关模块

`$ yum -y install python3 python3-devel`

### 更新 pip

`$ pip3 install --upgrade pip`

### 安装 gcc

uwsgi 需要 gcc 编译环境.

`$ yum -y install gcc`

### 安装 uwsgi

`$ pip install uwsgi`

## 修改相关配置

修改 nginx, uwsgi 相关配置.

### 修改 nginx 配置

修改 `/etc/nginx/nginx.conf` 配置如下:
```conf
server {
	location / {
		include uwsgi_params;
		uwsgi_pass 127.0.0.1:9000;
	}
}
```

### 修改 uwsgi 配置

新建 `/etc/uwsgi.ini` 配置如下:
```ini
[uwsgi]
master = true
vacuum = true
socket = 127.0.0.1:9000
pidfile = /var/run/uwsgi.pid
daemonize = /var/log/uwsgi.log
wsgi-file = /var/www/wsgi/index.py
```

## 测试及打包镜像

测试服务及打包镜像.

### 新建 index.py

新建 `/var/www/wsgi/index.py` 如下:
```python
def application(environ, start_response):
    start_response('200 OK', [('Content-Type', 'text/html')])
    return [b'<div>\
                <h1>WSGI</h1>\
                <h2>Contents</h2>\
                <ul>\
                    <li><a href="https://wsgi.readthedocs.io/en/latest/what.html">What is WSGI?</a></li>\
                    <li><a href="https://wsgi.readthedocs.io/en/latest/learn.html">Learn about WSGI</a></li>\
                    <li><a href="https://wsgi.readthedocs.io/en/latest/frameworks.html">Frameworks that run on WSGI</a></li>\
                    <li><a href="https://wsgi.readthedocs.io/en/latest/servers.html">Servers which support WSGI</a></li>\
                    <li><a href="https://wsgi.readthedocs.io/en/latest/applications.html">Applications that run on WSGI</a></li>\
                    <li><a href="https://wsgi.readthedocs.io/en/latest/libraries.html">Middleware and libraries for WSGI</a></li>\
                    <li><a href="https://wsgi.readthedocs.io/en/latest/testing.html">Testing tools for WSGI</a></li>\
                    <li><a href="https://wsgi.readthedocs.io/en/latest/presentations.html">Presentations about WSGI</a></li>\
                    <li><a href="https://wsgi.readthedocs.io/en/latest/specifications.html">Specifications related to WSGI</a></li>\
                    <li><a href="https://wsgi.readthedocs.io/en/latest/amendments-1.0.html">Amendments to WSGI 1.0</a></li>\
                    <li><a href="https://wsgi.readthedocs.io/en/latest/proposals-2.0.html">Proposals related to WSGI 2.0</a></li>\
                    <li><a href="https://wsgi.readthedocs.io/en/latest/python3.html">Python 3</a></li>\
                    <li><a href="https://wsgi.readthedocs.io/en/latest/definitions.html">Definitions of keys and classes</a></li>\
                </ul>\
            </div>']
```

### 配置自启服务

修改 `/etc/rc.d/rc.local` 配置如下:
```local
systemctl start nginx
systemctl start mysqld
uwsgi -i /etc/uwsgi.ini
```

赋予文件权限.

`$ chmod 755 /etc/rc.d/rc.local`

### 打包镜像

退出容器.

`$ exit`

将容器打包成镜像.

`$ docker commit wsgi akijoey/wsgi`

>镜像位于 Docker: https://hub.docker.com/r/akijoey/wsgi