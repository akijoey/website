---
title: Centos 搭建 L4D2 服务器
date: 2020-04-02
tags: Docker
categories: Common
keywords: Docker
description: Docker
cover: /img/cover_posts/csgo.jpg
top_img: /img/cover_posts/csgo.jpg
top: True
---

## 创建容器

原始镜像为 `centos:8`.

### 获取原始镜像

`$ docker pull centos`

### 创建容器

`$ docker run -d --net=host --privileged --name l4d2 centos /usr/sbin/init`

`--privileged` 启动 dbus-daemon
`/usr/sbin/init` 设置 CMD

### 进入容器

`$ docker exec -it l4d2 /bin/bash`

## 安装 steamcmd

steamcmd 即为 steam 的 cmd 版本.

### 更新 yum

`$ yum -y update`

### 安装 wget

`$ yum -y install wget`

### 安装 unzip

`$ yum -y install unzip`

### 下载 steamcmd

`$ wget http://media.steampowered.com/installer/steamcmd_linux.tar.gz`

### 解压 steamcmd

`$ tar -xvzf steamcmd_linux.tar.gz`

### 安装 32 位 lib 库

因为原始镜像为 64 位 centos.
需要安装 32 位 lib 库 glibc.i686 libstdc++.i686.

`$ yum -y install glibc.i686 libstdc++.i686 libcurl.i686`

## 安装 l4d2 server

在 steamcmd 客户端中下载或更新 csgo server.

### 启动 steamcmd

运行脚本 `steamcmd.sh` 启动 steamcmd.

`$ ./steamcmd.sh`

### 匿名登录 steamcmd

`> login anonymous`

> 某些内核版本可能导致 `FAILED. Timed out`

### 设置安装路径

`> force_install_dir /home/l4d2/`

### 下载 csgo server

`> app_update 222860 validate`

至此, 普通的 l4d2 服务器搭建完成.

## 安装相关插件

安装 sourcemod 和 mmsource.

### 安装 sourcemod

下载 sourcemod.

`$ wget https://sm.alliedmods.net/smdrop/1.10/sourcemod-1.10.0-git6460-linux.tar.gz`

解压 sourcemod 到 `/home/l4d2/left4dead2` 并删除压缩包.

`$ tar -xzvf sourcemod-1.10.0-git6460-linux.tar.gz -C /home/l4d2/left4dead2 && rm -f sourcemod-1.10.0-git6460-linux.tar.gz`

### 安装 mmsource

下载 mmsource.

`$ wget https://mms.alliedmods.net/mmsdrop/1.10/mmsource-1.10.7-git971-linux.tar.gz`

解压 mmsource 到 `/home/l4d2/left4dead2` 并删除压缩包.

`$ tar -xzvf mmsource-1.10.7-git971-linux.tar.gz -C /home/l4d2/left4dead2 && rm -f mmsource-1.10.7-git971-linux.tar.gz`

## 修改相关配置

修改 l4d2 server 相关配置.

### 服务器官网

将 `/home/l4d2/left4dead2/motd.txt` 修改为服务器官网
```txt
https://akijoey.com
```

### 服务器配置

修改服务器配置 `/home/l4d2/left4dead2/cfg/server.cfg`

### 数据库配置

修改数据库配置 `/home/l4d2/left4dead2/addons/sourcemod/configs/databases.cfg`

### 地图列表配置

修改地图列表文件 `/home/l4d2/left4dead2/maplist.txt` 和 `/home/l4d2/left4dead2/mapcycle.txt`

### 设置 sourcemod 管理员权限

`$ echo '"Your steamID" "99:z"' >> /home/l4d2/left4dead2/addons/sourcemod/configs/admins_simple.ini`

## http 服务搭建

使用 nginx 搭建 http 服务.
用于地图, 模型, 声音等资源的下载.

### 安装 nginx

`$ yum -y install nginx`

### 修改配置

修改 http 服务器根目录为 l4d2 服务器根目录.

修改 `/etc/nginx/nginx.conf` 配置文件:
```conf
root /home/l4d2/left4dead2;
```

## 启动服务

启动相关服务.

### 启动 nginx

`$ systemctl start nginx`

### 启动 l4d2 server

`$ /home/l4d2/srcds_run +port 27015 +exec server.cfg +map MAPNAME`

MAPNAME 为 `/home/l4d2/left4dead2/maps` 目录下的地图名.
可以通过创意工坊下载地图, 再将地图移至 maps 目录下.

`+host_workshop_collection COLLECTIONID` 下载创意工坊地图集
COLLECTIONID 为地图集 id.

### 配置自启服务

修改 `/etc/rc.d/rc.local` 配置如下:
```local
systemctl start nginx
/home/l4d2/srcds_run +port 27015 +exec server.cfg
```

赋予文件权限.

`$ chmod 755 /etc/rc.d/rc.local`

### 打包镜像

退出容器.

`$ exit`

将容器打包成镜像.

`$ docker commit l4d2 akijoey/l4d2`
