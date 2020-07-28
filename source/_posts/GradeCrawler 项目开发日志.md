---
title: GradeCrawler 项目开发日志
date: 2019-11-03
tags: [Python, Pip, Django, React, Npm]
categories: Backend
keywords: Django
description: Django
cover: /img/cover_posts/django.jpg
top_img: /img/cover_posts/django.jpg
---
# GradeCrawler 成绩爬虫系统

基于 Django + React 开发.
本文为该项目的具体开发日志.

## 前后端项目整合

分别创建前后端项目, 然后进行整合.

### 安装 Django 模块

`$ pip install Django`

### 新建 Django 项目

`$ django-admin startproject server`

### 启动 Django 本地调试环境

`$ python manage.py runserver`

默认监听端口为 8000.
可在 http://localhost:8000 访问项目.

### 新建 app

`$ django-admin startapp app`

### 全局安装 React 脚手架

`$ npm install -g create-react-app`

### 新建 React 项目

使用 create-react-app 脚手架搭建项目并安装相关依赖.

`$ create-react-app frontend`

### 启动 React 本地调试环境

`$ npm start`

默认监听端口为 3000.
可在 http://localhost:3000 访问项目.

### 目录结构

创建 GradeCrawler 目录.
将 server 项目和 frontend 项目移动到 GradeCrawler 目录中.

目录结构如下:
```
GradeCrawler/
|-- app/
|-- node_modules/
|-- public/
|-- server/
|-- src/
|-- .gitignore
|-- manage.py
|-- package.json
|-- package-lock.json
`-- README.md
```

## React 初始化

添加其他前端依赖, 配置静态资源.

### 安装 Antd 依赖

组件库使用 Antd.

`$ npm i antd -S`

### 安装 Axios 依赖

请求库使用 Axios.

`$ npm i axios -S`

### 新建组件

新建组件目录用于存放 React 组件.

- 创建 `src/components` 目录

新建 React 组件文件.

- 创建 `src/components/header.js` 组件
- 创建 `src/components/content.js` 组件
- 创建 `src/components/footer.js` 组件

### 新建样式

新建 Css 样式文件.

- 创建 `src/components/header.css` 样式
- 创建 `src/components/content.css` 样式
- 创建 `src/components/footer.css` 样式

### 配置 App.js

修改 `src/App.js` 文件内容如下:
```js
import React from 'react';
// import logo from './logo.svg';
import './App.css';

import Header from './components/header';
import Content from './components/content';
import Footer from './components/footer';

import 'antd/dist/antd.css';

function App() {
    return (
        <div className="App">
            <Header />
            <Content />
            <Footer />
        </div>
    );
}

export default App;
```

### 配置静态资源

Django 中的静态资源为统一路由管理, 因此要放在同一目录下.

将 `public` 下的静态资源移动到 `public/static` 目录中.

- 移动 `public/logo192.png` 到 `public/static/logo192.png`
- 移动 `public/favicon.ico` 到 `public/static/favicon.ico`
- 移动 `public/manifest.json` 到 `public/static/manifest.json`

修改 `public/index.html` 视图中的静态资源路径:
```html
<link rel="icon" href="%PUBLIC_URL%/static/favicon.ico" />

<link rel="apple-touch-icon" href="/static/logo192.png" />

<link rel="manifest" href="%PUBLIC_URL%/static/manifest.json" />
```

### 打包前端项目

`$ npm run build`

## Django 初始化

对项目进行初始化设置.

### 配置 settings.py

配置 templates 路径和 static 路径.

修改 `server/settings.py` 中的 `ALLOWED_HOSTS`项, `TEMPLATES` 项和 `STATICFILES_DIRS` 项.
```python
ALLOWED_HOSTS = ['*']

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR, 'build'],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

STATICFILES_DIRS = (
    os.path.join(BASE_DIR, 'build/static'),
)
```

### 添加 Controller

在 `app/views.py` 中添加 Web 控制器
```python
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.

def index(request):
	return render(request, 'index.html')
```

### 路由配置

修改 `server/urls.py` 中的 `urlpatterns` 路由:
```python
# from django.contrib import admin
from django.urls import path

from app import views

urlpatterns = [
    # path('admin/', admin.site.urls),
    path('', views.index)
]
```

## React 组件开发

略

## Django 实现后端接口

Django 提供 API 接口.

### 导入 Crawler

在项目中新建 Crawler 模块.

- 创建 `/crawler` 目录

新建 Crawler 模块文件.

- 创建 `/crawler/__init__.py` 标识文件
- 创建 `/crawler/login.py` 模块文件
- 导入 `/crawler/hex2b64.py` 模块文件
- 导入 `/crawler/RSAJS.py` 模块文件

`login.py` 爬虫内容如下:
```python
# -*- coding=utf-8 -*-
import requests
from time import time
from lxml import etree
from crawler.hex2b64 import HB64
from crawler.RSAJS import RSAKey

class Login():
	def __init__(self, username, password, year, term):	# 初始化数据
		self.session = requests.Session()
		self.username = username
		self.password = password
		self.year = year
		self.term = str(term * term * 3)
		self.timestamp = str(time() * 1000)
		self.url_key = "http://jwsys.gdpu.edu.cn/xtgl/login_getPublicKey.html?time="	# 获取公钥参数的 url
		self.url_login = "http://jwsys.gdpu.edu.cn/xtgl/login_slogin.html?language=zh_CN&_t="	# 登录主页的 url
		self.url_grade = "http://jwsys.gdpu.edu.cn/cjcx/cjcx_cxDgXscj.html?doType=query&gnmkdm=N305005"	# 查询成绩的 url

	def getCsrfToken(self):	# 获取 csrftoken
		response = self.session.get(self.url_login + self.timestamp)
		lxml = etree.HTML(response.content.decode("utf-8"))
		self.csrftoken = lxml.xpath("//input[@id='csrftoken']/@value")[0]

	def getPublicKey(self):	# 获取公钥参数
		response = self.session.get(self.url_key + self.timestamp).json()
		self.modulus = response["modulus"]
		self.exponent = response["exponent"]

	def getEnPassword(self):	# 生成公钥进行加密
		rsaKey = RSAKey()
		rsaKey.setPublic(HB64().b642hex(self.modulus), HB64().b642hex(self.exponent))
		self.enPassword = HB64().hex2b64(rsaKey.encrypt(self.password))

	def loginHome(self):	# 登录主页
		self.getCsrfToken()
		self.getPublicKey()
		self.getEnPassword()
		data = [("csrftoken", self.csrftoken), ("yhm", self.username), ("mm", self.enPassword), ("mm", self.enPassword)]
		response = self.session.post(self.url_login + self.timestamp, data = data)

	def getGrade(self):	# 获取成绩
		self.loginHome()
		data = [("xnm", self.year), ("xqm", self.term), ("_search", "false"), ("nd", self.timestamp), ("queryModel.showCount", "15"), ("queryModel.currentPage", "1"), ("queryModel.sortName", ""), ("queryModel.sortOrder", "asc"), ("time", "0")]
		response = self.session.post(self.url_grade, data = data).json()
		return response
```

### 添加 Controller

请求头 `Content-Type` 为 `application/json` 时
Django 不支持 request.POST.get() 方法
可以通过 request.body 获取参数.

`@csrf_exempt` 装饰器用于忽略 csrf 校验.

在 `app/views.py` 中添加 API 控制器
```python
from django.shortcuts import render
from django.http import HttpResponse
from django.views.decorators.csrf import csrf_exempt

from crawler.login import Login
import json

# Create your views here.

def index(request):
	return render(request, 'index.html')

@csrf_exempt
def search(request):
	if request.method == 'POST':
		username = json.loads(request.body).get('username')
		password = json.loads(request.body).get('password')
		year = json.loads(request.body).get('year')
		term = json.loads(request.body).get('term')
		crawler = Login(username, password, year, int(term))
		response = crawler.getGrade()
		data = []
		for item in response['items']:
			data.append(
				{
					'name': item['kcmc'],
					'type': item['kcxzmc'],
					'credit': item['xf'],
					'grade': item['cj'],
					'gpa': item['jd']
				}
			)
		return HttpResponse(json.dumps(data), content_type='application/json')
```

### 路由配置

在 `server/urls.py` 中添加 `/search` 路由:
```python
# from django.contrib import admin
from django.urls import path

from app import views

urlpatterns = [
    # path('admin/', admin.site.urls),
    path('', views.index),
    path('search', views.search)
]
```

## React 实现前后端交互

Axios 请求后端接口, 实现前后端交互.

### Axios 请求后端接口

在 `content.js` 组件的 handleClick 方法中发送 post 请求:
```javascript
axios.post('http://localhost:8000/search', {
	username: this.state.username,
	password: this.state.password,
	year: this.state.year,
	term: this.state.term
})
.then(response => {
	this.setState({
		data: response.data
	});
	message.success('Search success.');
	console.log(response);
})
.catch(error => {
	message.error('Search error.');
	console.log(error);
});
```

## Docker 部署

原始镜像为 `akijoey/django`.

### 创建容器

获取原始镜像.

`$ docker pull akijoey/django`

创建容器.

`$ docker run -d -p 8000:80 --privileged --name django akijoey/django /usr/sbin/init`

`--privileged` 启动 dbus-daemon
`/usr/sbin/init` 设置 CMD

进入容器.

`$ docker exec -it django /bin/bash`

### 项目部署

清空 `/var/www/django` 目录

`$ rm -rf /var/www/django`

在 /var/www 下获取项目

`$ git clone https://github.com/AkiJoey/GradeCrawler.git /var/www/django`

### 项目初始化

安装 python 相关模块.

`$ pip install requests lxml rsa`

修改 `/etc/nginx/nginx.conf` 配置如下:
```conf
server {
	location /static {
		alias /var/www/django/build/static;
	}
}
```

修改 `/etc/uwsgi.ini` 配置如下:
```ini
wsgi-file = server/wsgi.py
```

非本地部署需要修改接口 url

`$ sed -i 's/localhost/xxx.xxx.xxx.xxx/g' /var/www/django/build/static/js/main.xxxxxxxx.chunk.js`

重启 Nginx 服务

`$ nginx -s reload`

重启 uWSGI 服务

`$ uwsgi --reload /var/run/uwsgi.pid`

至此, 本项目基本功能开发及部署完成.

>项目位于 Github: https://github.com/AkiJoey/GradeCrawler