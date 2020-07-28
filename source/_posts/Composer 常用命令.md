---
title: Composer 常用命令
date: 2019-10-01
tags: Composer
categories: Backend
keywords: Composer
description: Composer
cover: /img/cover_posts/php.jpg
top_img: /img/cover_posts/php.jpg
---
## 查看版本

`$ composer -V`

## 安装依赖

`$ composer require`

composer require 会将依赖信息添加到 composer.json 文件中, 然后进行安装.

`$ composer install`

composer require 是从 composer.json 或 composer.lock 文件中提取依赖信息，然后进行安装.

`--ignore-platform-reqs` 忽略版本匹配.

## 更新依赖

`$ composer update`

## 卸载依赖

`$ composer remove`

## 查看依赖

`$ composer show`

## 查看配置

`$ composer config`

## 更换镜像

`$ composer config -g repo.packagist composer https://packagist.phpcomposer.com`

修改 composer 的全局配置文件的镜像为国内镜像 [Packagist / Composer](https://pkg.phpcomposer.com/)

`$ composer config -g --unset repos.packagist`

恢复官方镜像