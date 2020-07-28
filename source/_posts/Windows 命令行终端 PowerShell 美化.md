---
title: Windows 命令行终端 PowerShell 美化
date: 2020-04-15
tags: PowerShell
categories: Common
keywords: PowerShell
description: PowerShell
cover: /img/cover_posts/PowerShell.jpg
top_img: /img/cover_posts/PowerShell.jpg
---
# Windows 命令行终端 PowerShell 美化

Windows 默认的 PowerShell 终端有点丑.
因此使用微软新发布的 Windows Terminal.

## 字体改造

需要等宽字体来替换掉默认的字体.
这里安装 Powerline 的字体库.

### 下载 Powerline 字体库

`git clone https://github.com/powerline/fonts.git --depth=1`

### 安装 Powerline 字体库

`./install.ps1`

## 文件配色

文件列表的配色过于单一.
那就让他变得五彩缤纷吧.
以管理员权限打开 PowerShell 安装 module.

### 安装 PSColor

具体文件类型的配色是可配置的, 详见 GitHub.

> GitHub page: [https://github.com/Davlind/PSColor](https://github.com/Davlind/PSColor)

`Install-Module PSColor -Scope CurrentUser`

## ScreenFetch

终端美化怎么能少了 screenfetch.

### 安装 windows-screenfetch

`Install-Module windows-screenfetch -Scope CurrentUser`

## 安装 oh-my-posh

类似 Linux 下配置 zsh 的 `oh-my-zsh`.
安装 `oh-my-posh` 相关 PowerShell module.

### 安装 posh-git

`Install-Module posh-git -Scope CurrentUser`

### 安装 oh-my-posh

`Install-Module oh-my-posh -Scope CurrentUser`

## 配置 oh-my-zsh

类似 Linux bash 中的 `.bashrc`.
需要新建一个配置文件 `$PROFILE`.

### 检测并初始化 Profile 文件

`if (!(Test-Path -Path $PROFILE )) { New-Item -Type File -Path $PROFILE -Force }`

### 修改配置文件

`notepad $PROFILE`

在 notepad 中修改配置:
```ps1
Import-Module PSColor
Import-Module posh-git
Import-Module oh-my-posh
Set-Theme PowerLine
```
