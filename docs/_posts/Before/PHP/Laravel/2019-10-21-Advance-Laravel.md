---
title: 用 Laravel 构建 JSON API
categories: 
  - PHP
description: RESTful 风格 API 设计原则与最佳实践
keywords: Laravel,
date: 2019-10-21 17:24:16
permalink: /pages/2ae1ef/
sidebar: auto
tags: 
  - 
author: 
  name: Xueliang
  link: https://github.com/Human0722
---
> RESTful 风格 API 设计原则与最佳实践

<span class="ec ec-sneezing-face"></span><span class="ec ec-sneezing-face"></span><span class="ec ec-sneezing-face"></span>
基( di )础( ji )的 Laravel 知识点 [传送门](https://human0722.github.io/wiki/Laravel/)

## 快速构建 RESTful API 
> Laravel 可以通过 资源控制器(resource)和路由快速构建遵循 REST 规范的路由。  

1. 首先创建资源控制器:
```php
php artisan make:controller TaskController --resource
```
2. 然后在路由文件中注册资源控制器的路由:
```php
Route::resource('tasks', 'TaskController');
```
通过上面两步，就构建好了以下API:
![Resource路由表](/images/laravel/restful-api.jpg)
3. 编辑资源文件，删除掉 ```show``` 和 ```edit``` 两个方法, API 接口用不到展示表单。
4. 得益于 Laravel 对 JSON 的友好支持, 即使在成员方法中返回对象、数组, 底层也会自动转为 JSON 格式。
```php
    public function index()
    {
        return User::where('id', 1)->get();
    }
```
![测试JSON](/images/laravel/advanceLaravel001.jpg)
5. 自定义 JSON 数据组成格式
