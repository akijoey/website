---
title: BulletinBoard 项目开发日志
date: 2019-12-25
tags: [Php, Composer, Laravel, Vue, Npm]
categories: Backend
keywords: Laravel
description: Laravel
cover: /img/cover_posts/laravel.jpg
top_img: /img/cover_posts/laravel.jpg
---
# BulletinBoard 简易论坛系统

基于 Laravel + Vue 开发.
本文为该项目的具体开发日志.

## 前后端项目整合

Laravel 项目已经整合了部分前端资源.
只需要安装前端依赖即可.

### 全局安装 Laravel 脚手架

`$ composer global require "laravel/installer"`

### 新建 Laravel 项目

`$ laravel new BulletinBoard`

### 安装 composer 依赖

`$ composer install`

安装 Laravel 项目依赖于 `/vendor` 目录.

### 配置全局环境变量 .env

将项目目录下的 `.env.example` 文件另存为 `.env`, 然后生成 APP_KEY

`$ php artisan key:generate`

### 启动 Laravel 本地调试环境

`$ php artisan serve`

默认监听端口为 8000.
可在 http://localhost:8000 访问项目.

### 安装 node 依赖

`$ npm install`

安装 node 项目依赖于 `/node_modules` 目录.

### 安装 vue 依赖

`$ npm i vue -D`

### 目录结构

目录结构如下:
```
BulletinBoard/
|-- app/
|-- bootstrap/
|-- config/
|-- database/
|-- node_modules/
|-- public/
|-- resources/
|-- routes/
|-- storage/
|-- tests/
|-- vendor/
|-- .editorconfig
|-- .env
|-- .env.example
|-- .gitattributes
|-- .gitignore
|-- .styleci.yml
|-- artisan
|-- composer.json
|-- composer.lock
|-- package.json
|-- package-lock.json
|-- phpunit.xml
|-- server.php
`-- webpack.mix.js
```

## Vue 初始化

添加其他前端依赖, 修改配置文件.

### 安装 Element 依赖

组件库使用 Element.

`$ npm i element-ui -S`

### 新建样式

新建样式目录用于存放 Scss 样式.

- 创建 `/resources/sass/components` 目录

新建 Scss 样式文件.

- 创建 `/resources/sass/components/header.scss` 样式
- 创建 `/resources/sass/components/content.scss` 样式
- 创建 `/resources/sass/components/footer.scss` 样式

### 新建组件

新建组件目录用于存放 Vue 组件.

- 创建 `/resources/js/components` 目录

新建 Vue 组件文件.

- 创建 `/resources/js/components/header.vue` 组件
- 创建 `/resources/js/components/content.vue` 组件
- 创建 `/resources/js/components/footer.vue` 组件

### 配置 app.js

修改 `/resources/assets/js/app.js` 文件内容如下:
```js
require('./bootstrap');

/**
 * Next we will register the CSRF Token as a common header with Axios so that
 * all outgoing HTTP requests automatically have it attached. This is just
 * a simple convenience so we don't have to attach every token manually.
 */

let token = document.head.querySelector('meta[name="csrf-token"]');

if (token) {
	window.axios.defaults.headers.common['X-CSRF-TOKEN'] = token.content;
} else {
	console.error('CSRF token not found: https://laravel.com/docs/csrf#csrf-x-csrf-token');
}

window.Vue = require('vue');

/**
 * Next, we will create a fresh Vue application instance and attach it to
 * the page. Then, you may begin adding components to this application
 * or customize the JavaScript scaffolding to fit your unique needs.
 */

Vue.component('app-header', require('./components/header.vue').default);
Vue.component('app-content', require('./components/content.vue').default);
Vue.component('app-footer', require('./components/footer.vue').default);

import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI);

const app = new Vue({
	el: '#app'
});
```

## Laravel 初始化

项目搭建好之后, 进行初始化设置.

### 新建 View

由于构建的是单页面应用 (SPA).
所以整个应用中只需要一个可以展示 SPA 的视图.

- 创建 `/resources/views/app.blade.php` 视图

`app.blade.php` 视图内容如下:
```php
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta name="viewport" content="width=device-width, initial-scale=1.0">
		<meta name="csrf-token" content="{{ csrf_token() }}">
		<link href="{{ asset('css/app.css') }}" rel="stylesheet" type="text/css"/>
		<link rel="icon" type="image/x-icon" href="/favicon.ico">
		<title>BulletinBoard</title>
		<script type='text/javascript'>
			window.Laravel = <?php echo json_encode([
				'csrfToken' => csrf_token(),
			]); ?>
		</script>
	</head>
	<body>
		<div id="app">
			<app-header></app-header>
			<app-content></app-content>
			<app-footer></app-footer>
		</div>
		<script type="text/javascript" src="{{ asset('js/app.js') }}"></script>
	</body>
</html>
```

### 分离 Controller

基于 API 和 Web 分离控制器.

- 创建 `/app/Http/Controllers/API` 目录
- 创建 `/app/Http/Controllers/Web` 目录

创建一个 Web 控制器用于返回 `app.blade.php` 视图.

- 创建 `/app/Http/Controllers/Web/AppController.php` 控制器

`AppController.php` 控制器内容如下:
```php
<?php

namespace app\Http\Controllers\Web;

use App\Http\Controllers\Controller;

class AppController extends Controller
{
	public function getApp()
	{
		return view('app');
	}
}
```

### 路由配置

在 `/routes/web.php` 文件中移除自带的 `/` 路由并新增如下路由:
```php
Route::get('/', 'Web\AppController@getApp');
```

## Vue 组件开发

略

## Laravel 实现后端接口

Laravel 提供 API 接口.

### 数据库配置

连接 mysql 服务器.

`$ mysql -u root -p`

创建 laravel 数据库.

`> create database laravel;`

在 `.env` 中修改数据库配置:
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```

### 新建 Model

新建 Article 模型并通过 Eloquent ORM 管理.

`$ php artisan make:model Models/Article -m`

`-m`, `--migration` 同时创建数据库迁移文件 `{timestamp}_create_articles_table`

### 数据库迁移

在 `/database/migrations/{timestamp}_create_articles_table` 中编辑 `up()` 方法:
```php
public function up()
{
	Schema::create('articles', function (Blueprint $table) {
		$table->bigIncrements('id');
		$table->string('title');
		$table->string('tags');
		$table->text('content');
		$table->timestamps();
	});
}
```

通过数据库迁移文件创建数据表.

`$ php artisan migrate --path=database/migrations/{timestamp}_articles_table.php`

### 新建 Controller

创建一个 API 控制器并构建增删改查方法.

- 创建 `/app/Http/Controllers/API/ArticlesController.php` 控制器

`ArticlesController.php` 控制器内容如下:
```php
<?php

namespace app\Http\Controllers\API;

use App\Http\Controllers\Controller;
use App\Models\Article;
use Request;

class ArticlesController extends Controller
{
	/*
	|-------------------------------------------------------------------------------
	| Get All Articles
	|-------------------------------------------------------------------------------
	| URL:            /api/article/get
	| Method:         GET
	| Description:    Gets all of the articles in the application
	*/
	public function getArticle() {
		$articles = Article::all();
		foreach ($articles as $article)
			$article->tags = explode(',', $article->tags);
		return response()->json($articles, 201);
	}

	/*
	|-------------------------------------------------------------------------------
	| Adds a New Article
	|-------------------------------------------------------------------------------
	| URL:            /api/article/post
	| Method:         POST
	| Description:    Adds a new article to the application
	*/
	public function postArticle() {
		$article = new Article();
		$article->title = Request::get('title');
		$article->content = Request::get('content');
		$article->tags = join(',', Request::get('tags'));
		$article->save();
		$article->tags = explode(',', $article->tags);
		return response()->json($article, 201);
	}

	/*
	|-------------------------------------------------------------------------------
	| Delete An Individual Article
	|-------------------------------------------------------------------------------
	| URL:            /api/article/delete
	| Method:         DELETE
	| Description:    Deletes an individual article
	*/
	public function deleteArticle() {
		$article = Article::find(Request::get('id'))->delete();
		return response()->json($article, 201);
	}
}
```

### 路由配置

在 `/routes/api.php` 文件中移除自带的 `/user` 路由并新增如下路由:
```php
Route::get('/article/get', 'API\ArticlesController@getArticle');

Route::post('/article/post', 'API\ArticlesController@postArticle');

Route::delete('/article/delete', 'API\ArticlesController@deleteArticle');
```

## Vue 实现前后端交互

Axios 请求后端接口, 实现前后端交互.

### Axios 请求后端接口

在 `header.vue` 组件的 formPost 方法中发送 post 请求:
```javascript
axios.post('http://localhost:8000/api/article/post', {
	title: this.title,
	tags: this.tags,
	content: this.content.replace(/\r\n/g, '<br/>').replace(/\n/g, '<br/>').replace(/\s/g, '&nbsp;')
})
.then(response => {
	this.$emit('insert', response.data);
	this.$message.success('Insert success.');
	console.log(response);
	initialize();
})
.catch(error => {
	this.$message.error('Insert error.');
	console.log(error);
});
```

在 `content.vue` 组件的 created 生命周期中发送 get 请求:
```javascript
axios.get('http://localhost:8000/api/article/get')
.then(response => {
	this.articles = response.data;
	console.log(response);
})
.catch(error => {
	console.log(error);
});
```

在 `content.vue` 组件的 articleDelete 方法中发送 delete 请求:
```javascript
axios.delete('http://localhost:8000/api/article/delete?id=' + this.articles[index].id)
.then(response => {
	this.articles.splice(index, 1);
	this.$message.success('Delete success.');
	console.log(response);
})
.catch(error => {
	this.$message.error('Delete error.');
	console.log(error);
});
```

### Vue 组件间数据传递

由于本项目逻辑比较简单, 不使用 Vuex.
可以通过父子组件之间的传值实现数据传递.

将 `header.vue` 中 post 请求的 response 传给父组件:
```javascript
this.$emit('insert', response.data);
```

父组件接收子组件 `header.vue` 传递的数据.
再将数据传递给子组件 `content.vue`.

修改 `app.js` 中的 vue 实例:
```javascript
const app = new Vue({
	el: '#app',
	data: {
		article: null
	},
	methods: {
		insert(article) {
			this.article = article;
		}
	}
});
```

修改 `app.blade.php` 视图:
```php
<div id="app">
	<app-header @insert="insert"></app-header>
	<app-content :article="article"></app-content>
	<app-footer></app-footer>
</div>
```

在 `content.vue` 中接收并监听父组件传递的数据:
```javascript
props: ['article'],
watch: {
	article(article) {
		this.articles.push(article);
	}
},
```

## Docker 部署

原始镜像为 `akijoey/laravel`.

### 创建容器

获取原始镜像.

`$ docker pull akijoey/laravel`

创建容器.

`$ docker run -d -p 8000:80 --privileged --name laravel akijoey/laravel /usr/sbin/init`

`--privileged` 启动 dbus-daemon
`/usr/sbin/init` 设置 CMD

进入容器.

`$ docker exec -it laravel /bin/bash`

### 项目部署

清空 `/var/www/laravel` 目录

`$ rm -rf /var/www/laravel`

在 `/var/www` 下获取项目

`$ git clone https://github.com/AkiJoey/BulletinBoard.git /var/www/laravel`

连接 mysql 服务器.

`$ mysql -u root`

创建 laravel 数据库.

`> create database laravel;`

### 项目初始化

进入项目目录

`$ cd /var/www/laravel`

安装 composer 依赖

`$ composer install --ignore-platform-reqs`

`--ignore-platform-reqs` 忽略版本匹配.

生成 `.env`

`$ cp .env.example .env`

生成 APP_KEY

`$ php artisan key:generate`

在 `.env` 中修改数据库配置:
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```

数据库迁移

`$ php artisan migrate`

赋予目录权限

`$ chmod -R 777 /var/www/laravel`

非本地部署需要修改接口 url

`$ sed -i 's/localhost/xxx.xxx.xxx.xxx/g' /var/www/laravel/public/static/js/app.js`

重启 Nginx 服务

`$ nginx -s reload`

至此, 本项目基本功能开发及部署完成.

>项目位于 Github: https://github.com/AkiJoey/BulletinBoard