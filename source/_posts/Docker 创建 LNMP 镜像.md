---
title: Docker 创建 LNMP 镜像
date: 2019-12-24
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

`$ docker run -d -p 8080:80 --privileged --name lnmp centos /usr/sbin/init`

`--privileged` 启动 dbus-daemon
`/usr/sbin/init` 设置 CMD

### 进入容器

`$ docker exec -it lnmp /bin/bash`

## 安装 LMNP

安装 nginx, mysql, php 及相关扩展.

### 更新 yum

`$ yum -y update`

### 安装 nginx

`$ yum -y install nginx`

### 安装 mysql

`$ yum -y install mysql-server`

### 安装 php 及相关扩展

`$ yum -y install php php-pdo php-mysqlnd`

## 修改相关配置

修改 nginx, php-fpm 相关配置.

### 修改 nginx 配置

修改 `/etc/nginx/nginx.conf` 配置如下:
```conf
server {
	root	/var/www/html;
	index	index.php index.html index.htm;

	location ~ \.php$ {
		include fastcgi.conf;
		fastcgi_pass 127.0.0.1:9000;
	}
}
```

修改 `/etc/nginx/conf.d/php-fpm.conf` 配置如下:
```conf
upstream php-fpm {
        server 127.0.0.1:9000;
}
```

### 修改 php 配置

修改 `/etc/php-fpm.d/www.conf` 配置如下:
```conf
listen = 127.0.0.1:9000
```

## 测试及打包镜像

测试服务及打包镜像.

### 新建 index.php

新建 `/var/www/html/index.php` 如下:
```php
<?php
	phpinfo();
```

### 配置自启服务

修改 `/etc/rc.d/rc.local` 配置如下:
```local
systemctl start nginx
systemctl start mysqld
```

赋予文件权限.

`$ chmod 755 /etc/rc.d/rc.local`

### 打包镜像

退出容器.

`$ exit`

将容器打包成镜像.

`$ docker commit lnmp akijoey/lnmp`

>镜像位于 Docker: https://hub.docker.com/r/akijoey/lnmp