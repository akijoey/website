---
title: Git 常用命令
date: 2019-10-01
tags: Git
categories: Common
keywords: Git
description: Git
cover: /img/cover_posts/git.jpg
top_img: /img/cover_posts/git.jpg
---
## 查看版本

`$ git --version`

## 设置全局配置

`$ git config --global user.name "userName"`

设置全局用户名

`$ git config --global user.email "userEmail"`

设置全局邮箱

`$ git config --global url."https://".insteadOf git://`

设置 https 协议代替 git 协议 (避免防火墙阻止 git)

## 查看全局配置

`$ git config --global -l`

## 创建版本库

`$ git init`

## 将工作区文件添加到暂存区

`$ git add .`

## 将暂存区文件提交到本地仓库

`$ git commit -m "default commit"`

## 查看提交历史

`$ git log`

查看提交次数及提交者

`$ git log | grep "^Author: " | awk '{print $2}' | sort | uniq -c | sort -k1,1nr`

## 查看状态

`$ git status`

## 查看改动

`$ git diff`

## 从远程仓库克隆文件到本地

`$ git clone <url>`

url 可为 http 协议或 ssh(git) 协议

## 查看远程仓库

`$ git remote -v`

## 添加远程仓库关联

`$ git remote add <name> <url>`

name 一般默认为 origin

## 修改远程仓库关联

`$ git remote set-url <name> <url>`

## 删除远程仓库关联

`$ git remote remove <name>`

## 将本地仓库推送到远程仓库

`$ git push origin master`

-f 强制推送
