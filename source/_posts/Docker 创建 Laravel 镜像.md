---
title: Docker 创建 Laravel 镜像
date: 2019-12-24
tags: Docker
categories: Backend
keywords: Docker
description: Docker
cover: /img/cover_posts/docker.jpg
top_img: /img/cover_posts/docker.jpg
---
## 创建容器

原始镜像为 `akijoey/lnmp`.

### 获取原始镜像

`$ docker pull akijoey/lnmp`

### 创建容器

`$ docker run -d -p 8000:80 --privileged --name laravel akijoey/lnmp /usr/sbin/init`

`--privileged` 启动 dbus-daemon
`/usr/sbin/init` 设置 CMD

### 进入容器

`$ docker exec -it laravel /bin/bash`

## 安装相关环境

安装 git, composer 及相关扩展.

### 更新 yum

`$ yum -y update`

### 安装 git

`$ yum -y install git`

在 `/var/www` 下获取项目.

### 安装 zip

`$ yum -y install zip`

用于安装项目依赖.

### 安装 php-json 扩展

`$ yum -y install php-json`

用于安装 composer.

### 安装 composer

下载安装文件.

`$ php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"`

执行安装文件.

`$ php composer-setup.php`

删除安装文件.

`$ php -r "unlink('composer-setup.php');"`

将 `composer.phar` 移动到 `/usr/bin` 下.

`$ mv composer.phar /usr/bin/composer`

## 修改相关配置

修改 nginx 相关配置.

### 修改 nginx.conf

修改 `/etc/nginx/nginx.conf` 配置如下:
```conf
server {
	root	/var/www/laravel/public;

	location / {
		try_files $uri $uri/ /index.php?$query_string;
	}
}
```

## 测试及打包镜像

测试服务及打包镜像.

### 新建 index.php

将 `/var/www/html/index.php` 移动到 `/var/www/laravel/public/index.php`

`$ mv /var/www/html/index.php /var/www/laravel/public/index.php`

### 打包镜像

退出容器.

`$ exit`

将容器打包成镜像.

`$ docker commit laravel akijoey/laravel`

>镜像位于 Docker: https://hub.docker.com/r/akijoey/laravel