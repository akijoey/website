---
title: TimeControl 项目开发日志
date: 2020-01-22
tags: [Ruby, Gem, Rails, Angular, Npm]
categories: Backend
keywords: Rails
description: Rails
cover: /img/cover_posts/rails.jpg
top_img: /img/cover_posts/rails.jpg
---
# TimeControl 时间管理系统

基于 Rails + Angular 开发.
本文为该项目的具体开发日志.

## 前后端项目整合

Rails 项目已经整合了部分前端资源.
只需要安装前端依赖即可.

### 安装 Rails 依赖

`$ gem install rails`

### 新建 Rails 项目

新建 Rails 项目, 数据库使用 mysql.

`$ rails new TimeControl -d mysql`

### 安装 gem 依赖

`$ bundle install`

### 启动 Rails 本地调试环境

`$ rails server`

默认监听端口为 3000.
可在 http://localhost:3000 访问项目.

### 安装 node 依赖

`$ npm install`

安装 node 项目依赖于 `/node_modules` 目录.

### 安装 angular 依赖

`$ rails webpacker:install:angular`

### 目录结构

目录结构如下:
```
TimeControl/
|-- app/
|-- bin/
|-- config/
|-- db/
|-- lib/
|-- log/
|-- node_modules/
|-- public/
|-- storage/
|-- test/
|-- tmp/
|-- vendor/
|-- .browserslistrc
|-- .gitignore
|-- .ruby-version
|-- babel.config.js
|-- config.ru
|-- Gemfile
|-- Gemfile.lock
|-- package.json
|-- postcss.config.js
|-- Rakefile
|-- README.md
|-- tsconfig.json
`-- yarn.lock
```

## Angular 初始化

添加其他前端依赖, 修改配置文件.

### 安装 Material 依赖

组件库使用 Material.

`$ yarn add @angular/material @angular/animations @angular/cdk @angular/forms hammerjs -D`

`--dev` or `-D` install package in your `devDependencies`.

### 新建样式

在 `/app/assets/stylesheets` 目录下新建 Scss 样式文件.

- 创建 `/app/assets/stylesheets/header.component.scss` 样式
- 创建 `/app/assets/stylesheets/content.component.scss` 样式
- 创建 `/app/assets/stylesheets/footer.component.scss` 样式

### 新建组件

移除默认的 hello_angular 组件及 pack 配置.

- 将 `/app/javascript/hello_angular` 目录重命名为 `src`
- 移除 `/app/javascript/packs/hello_angular.js`
- 移除 `/app/javascript/packs/hello_typescript.ts`

新建 `/app/javascript/packs/index.js` 内容如下:
```javascript
require('../src')
```

新建组件目录用于存放 Angular 组件.

- 创建 `/app/javascript/src/app/components` 目录

新建 Angular 组件文件.

- 创建 `/app/javascript/src/app/components/header.component.ts` 组件
- 创建 `/app/javascript/src/app/components/content.component.ts` 组件
- 创建 `/app/javascript/src/app/components/footer.component.ts` 组件

### 修改配置

修改 `/app/javascript/src/app/app.component.ts` 文件内容如下:
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
      <app-header></app-header>
      <app-content></app-content>
      <app-footer></app-footer>
    `,
  styleUrls: ['/app/assets/stylesheets/app.component.scss']
})
export class AppComponent {
  // name = 'Angular!';
}
```

修改 `/app/javascript/src/app/app.module.ts` 文件内容如下:
```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';
import { HeaderComponent } from './components/header.component';
import { ContentComponent } from './components/content.component';
import { FooterComponent } from './components/footer.component';

@NgModule({
  declarations: [
    AppComponent,
    HeaderComponent,
    ContentComponent,
    FooterComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## Rails 初始化

项目搭建好之后, 进行初始化设置.

### 生成 Controller

生成 Web 控制器 `/app/controllers/web_controller.rb`.

`$ rails generate controller web index --no-assets`

`--no-assets` 无静态资源.

同时生成视图 `/app/views/web/index.html.erb`.

修改 `/app/views/web/index.html.erb` 视图内容如下:
```erb
<app-root></app-root>
```

修改 `/app/views/layouts/application.html.erb` 视图模板内容如下:
```erb
<!DOCTYPE html>
<html>
  <head>
    <title>TimeControl</title>
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
    <%= javascript_pack_tag 'index' %>
  </head>

  <body>
    <%= yield %>
  </body>
</html>
```

### 路由配置

在 `/config/routes.rb` 文件中修改 web 路由:
```rb
root 'web#index'
```

## Angular 组件开发

略

## Rails 实现后端接口

Rails 提供 API 接口.

### 数据库配置

连接 mysql 服务器.

`$ mysql -u root -p`

创建 rails 数据库.

`> create database rails;`

在 `/config/database.yml` 中修改数据库配置:
```yml
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password:
  host: localhost

development:
  <<: *default
  database: rails
```

### 新建 Model

新建 Project 模型.

`$ rails generate model project name:string date:timestamp`

新建 Mission 模型.

`$ rails generate model mission name:string tags:string state:boolean`

### 数据库迁移

通过数据库迁移文件创建数据表.

`$ rake db:migrate`

### 数据初始化

修改 `/db/seeds.rb` 初始化文件如下:
```rb
Project.create({
	:name => nil,
	:date => nil
})
```

初始化 projects 表数据.

`$ rake db:seed`

### 新建 Controller

生成 API 控制器 `/app/controllers/api_controller.rb`.

`$ rails generate controller api --no-assets`

`api_controller.rb` 控制器内容如下:
```rb
class ApiController < ApplicationController

	# disable the CSRF token
	skip_before_action :verify_authenticity_token

	def getProject
		@project = Project.find(1)
		render json: @project, status: '200 OK'
	end

	def postName
		@project = Project.update(1, name: params[:name])
		render json: @project, status: '200 OK'
	end

	def postDate
		@project = Project.update(1, date: params[:date])
		render json: @project, status: '200 OK'
	end

	def getMissions
		@missions = Mission.all
		render json: @missions, status: '200 OK'
	end

	def getLast
		@mission = Mission.last
		render json: @mission, status: '200 OK'
	end

	def postMission
		@mission = Mission.create({
			name: params[:name],
			tags: params[:tags].join(','),
			state: false
		})
		render json: @mission, status: '200 OK'
	end

	def postState
		@mission = Mission.update(params[:id], state: params[:state])
		render json: @mission, status: '200 OK'
	end

	def deleteMission
		@mission = Mission.delete(params[:id])
		render json: @mission, status: '200 OK'
	end

end
```

### 路由配置

在 `/config/routes.rb` 文件中新增 api 路由:
```rb
get 'get/project' => 'api#getProject'
post 'post/name' => 'api#postName'
post 'post/date' => 'api#postDate'

get 'get/missions' => 'api#getMissions'
get 'get/last' => 'api#getLast'
post 'post/mission' => 'api#postMission'
post 'post/state' => 'api#postState'
delete 'delete/mission' => 'api#deleteMission'
```

## Angular 实现前后端交互

使用 `@angular/common/http` 中的 `HttpClient` 类, 实现前后端交互.

### 导入 HttpClientModule

在 `/app/javascript/src/app/app.module.ts` 中导入 `HttpClientModule` 模块:
```ts
import { NgModule }         from '@angular/core';
import { BrowserModule }    from '@angular/platform-browser';
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  imports: [
    BrowserModule,
    // import HttpClientModule after BrowserModule.
    HttpClientModule,
  ],
  declarations: [
    AppComponent,
  ],
  bootstrap: [ AppComponent ]
})
export class AppModule {}
```

与后端的交互都在 `content.component.ts` 组件中进行.

在 `/app/javascript/src/app/components/content.component.ts` 中注入 `HttpClient`:
```ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Injectable()
export class ContentComponent {
  constructor(private http: HttpClient) { }
}
```

### HttpClient 请求后端接口

在组件的 ngOnInit 生命周期中发送 get 请求获取所有信息:
```ts
this.http.get('http://localhost:3000/get/project')
.subscribe(
  response => {
    this.name = response.name;
    this.date = response.date;
    console.log(response);
  },
  error => {
    console.log(error);
  }
);
this.http.get('http://localhost:3000/get/missions')
.subscribe(
  response => {
    response.forEach(mission => {
      this.missions.push({
        id: mission.id,
        name: mission.name,
        tags: mission.tags.split(','),
        state: mission.state
      });
    });
    console.log(response);
  },
  error => {
    console.log(error);
  }
);
```

在 insertMission 方法中发送 get 请求:
```ts
this.http.get('http://localhost:3000/get/last')
.subscribe(
  response => {
    result.id = response.id;
    console.log(response);
  },
  error => {
    console.log(error);
  }
);
```

在 postName 方法中发送 post 请求:
```ts
this.http.post('http://localhost:3000/post/name', {
  name: this.name
})
.subscribe(
  response => {
    console.log(response);
  },
  error => {
    console.log(error);
  }
);
```

在 postDate 方法中发送 post 请求:
```ts
this.http.post('http://localhost:3000/post/date', {
  date: this.date
})
.subscribe(
  response => {
    console.log(response);
  },
  error => {
    console.log(error);
  }
);
```

在 postState 方法中发送 post 请求:
```ts
this.http.post('http://localhost:3000/post/state', {
  id: this.missions[index].id,
  state: !this.missions[index].state
})
.subscribe(
  response => {
    console.log(response);
  },
  error => {
    console.log(error);
  }
);
```

在 postMission 方法中发送 post 请求:
```ts
this.http.post('http://localhost:3000/post/mission', {
  name: this.name,
  tags: this.tags
})
.subscribe(
  response => {
    this.snackBar.open('✅ Insert', 'success', this.data.snackBarConfig);
    console.log(response);
  },
  error => {
    this.snackBar.open('❌ Insert', 'error', this.data.snackBarConfig);
    console.log(error);
  }
);
```

### Dialog 组件间数据传递

DialogComponent 组件是独立于 ContentComponent 的组件.
组件间共享数据需要通过数据传递.

将 `ContentComponent` 组件中 data 传给 `DialogComponent` 组件:
```ts
const dialogRef = this.dialog.open(DialogComponent, {
  data: {
    snackBarConfig: this.snackBarConfig
  }
});
```

`DialogComponent` 组件中使用 MAT_DIALOG_DATA 注入令牌接收 data.
```ts
import {MAT_DIALOG_DATA} from '@angular/material/dialog';

export class DialogComponent {
  constructor(@Inject(MAT_DIALOG_DATA) public data: any) { }
}
```

`DialogComponent` 组件中返回数据
```ts
[mat-dialog-close]="{id:id,name:name,tags:tags,state:false}"
```

`ContentComponent` 组件接收 `DialogComponent` 组件返回的数据
```ts
dialogRef.afterClosed().subscribe(result => { });
```

## Docker 部署

To be continued.

>项目位于 Github: https://github.com/AkiJoey/TimeControl