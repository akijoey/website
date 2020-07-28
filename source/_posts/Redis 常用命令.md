---
title: Redis 常用命令
date: 2020-05-21
tags: Redis
categories: Backend
keywords: Redis
description: Redis
cover: /img/cover_posts/redis.jpg
top_img: /img/cover_posts/redis.jpg
---
## 拉取 redis 镜像

`$ docker pull redis`

## 构建 redis 容器

`$ docker run -d -p 6379:6379 --name redis redis`

redis 默认端口为 6379

## 查看版本

`$ redis-cli -v`

## 启动 redis 服务

`$ redis-server`

## 连接 redis 服务器

`$ redis-cli -h <host> -p <port> -a <password>`

## 查看服务是否运行

`> ping`

## 数据库备份

`> save`

在 redis 根目录中创建 dump.rdb 备份文件

## 后台备份

`> bgsave`

后台备份不影响主进程

## 获取备份目录

`> config get dir`

只要将备份文件 dump.rdb 放到备份目录下, 重启 redis 即可恢复备份数据

## 退出 redis 客户端

`> exit`

## 关闭 redis 服务器

`> shutdown`

## 选择数据库

`> select <database>`

## 检查 key 是否存在

`> exists <key>`

## 设置 key 的值

`> set <key> <value>`

## 获取 key 的值

`> get <key>`

## 开启事务

`> multi`

## 执行事务

`> exec`

## 取消事务

`> discard`

## 查看密码

`> config get requirepass`

## 设置密码

`> config set requirepass <password>`

## 验证密码

`> auth <password>`

## 查看主从状态

`> info replication`
