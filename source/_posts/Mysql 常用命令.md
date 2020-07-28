---
title: Mysql 常用命令
date: 2019-12-09
tags: Mysql
categories: Backend
keywords: Mysql
description: Mysql
cover: /img/cover_posts/mysql.jpg
top_img: /img/cover_posts/mysql.jpg
---
## 拉取 mysql 镜像

`$ docker pull mysql`

## 构建 mysql 容器

`$ docker run -d -p 3306:3306 -p 33060:33060 --name mysql mysql`

mysql 默认端口为 3306, 数据库扩展接口的默认端口为 33060

## 查看版本

`$ mysql -V`

## 启动 mysql 服务

`$ mysqld`

## 连接 mysql 服务器

`$ mysql -u <username> -p`

## 数据库备份

`$ mysqldump -u <username> -p -B <database> > <database>.bak.sql`

`-B` 在备份文件中添加 `create database <database>` 和 `use <database>`

## 数据库还原

`$ mysql -u <username> -p < <database>.bak.sql`

## 数据库迁移

`$ mysqldump -u <username> -p <database> | mysql -h <host> -u <username> -p`

`-h` 服务器 ip 地址

## 退出 mysql 客户端

`> exit`

## 查看当前用户

`> select user();`

## 查看当前数据库

`> select database();`

## 选择数据库

`> use <database>`

## 显示数据库

`> show databases;`

## 创建数据库

`> create database <database>;`

## 删除数据库

`> drop database <database>;`

## 查看数据表

`> show tables;`

## 选择 mysql 数据库进行用户操作

`> use mysql`

## 查看 user 表

`> select host, user from user;`

## 创建用户

`> create user '<username>'@'<host>' identified by '<password>';`

## 修改用户密码

`> alter user '<username>'@'<host>' identified by '<password>';`

## 删除用户

`> drop user '<username>'@'<host>';`

## 查看用户权限

`> show grants for '<username>'@'<host>';`

## 授予用户权限

`> grant all privileges on <database>.* to '<username>'@'<host>';`

## 废除用户权限

`> revoke all privileges on <database>.* from '<username>'@'<host>';`

## 刷新权限

`> flush privileges;`
