---
title: Mongo 常用命令
date: 2020-05-21
tags: Mongo
categories: Backend
keywords: Mongo
description: Mongo
cover: /img/cover_posts/mongo.jpg
top_img: /img/cover_posts/mongo.jpg
---
## 拉取 mongo 镜像

`$ docker pull mongo`

## 构建 mongo 容器

`$ docker run -d -p 27017:27017 --name mongo mongo`

mongo 默认端口为 27017

## 查看版本

`$ mongo --version`

## 启动 mongo 服务

`$ mongod`

## 连接 mongo 服务器

`$ mongo <host>:<port>/<database> -u <username> -p <password>`

## 数据库备份

`$ mongodump -h <host> --port <port> -u <username> -p <password> -d <database> -c <collection> -o <database>.bak.json`

## 数据库还原

`$ mongorestore -h <host> --port <port> -u <username> -p <password> -d <database> <database>.bak.json`

## 退出 mongo 客户端

`> exit`

## 查看当前数据库

`> db`

## 查看当前数据库状态

`> db.stats()`

## 查看当前数据库下所有用户

`> show users`

## 选择数据库

`> use <database>`

若 database 不存在, 则创建 database

## 查看所有数据库

`> show dbs`

## 删除当前数据库

`> db.dropDatabase()`

## 创建集合

`> db.createCollection(<collection>)`

## 查看所有集合

`> show collections`

## 删除集合

`> db.<collection>.drop()`

## 查询文档

`> db.<collection>.find()`

## 插入文档

`> db.<collection>.insert(<document>)`
