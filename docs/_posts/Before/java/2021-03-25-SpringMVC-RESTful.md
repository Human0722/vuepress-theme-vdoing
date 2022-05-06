---
title: RESTful API 最佳实践
categories: 
  - Java
description: Standred RESTful api
keywords: SpringMVC
date: 2021-03-25 17:24:16
permalink: /pages/b7250d/
sidebar: auto
tags: 
  - 
author: 
  name: Xueliang
  link: https://github.com/Human0722
---

> Build Standred REST API

### RESTful是什么   
RESTful(Representational State Transfer), (资源)表现层状态转化。资源是网络上的一个实体，对应一个 URI. 表现层是资源的呈现方式，以 html、xml 等。状态转换，说的是由于 HTTP 协议是无状态协议，所有的状态都保存在服务器。综上，客户端想要操作服务器，就要让服务器端发生状态转换。而这种转换建立在表现层之上，所有叫 RESTful。  
通俗的讲，就是 HTTP 协议里面，四个表示操作的动词: GET POST PUT DELETE。其分别对应四种操作: GET 用来获取资源、POST 用来新建资源、PUT 用来更新资源、DELETE 用来删除资源。  
用这几种方式操作服务器， 这些方式就是 RESTful(adj) 的。  
### RESTful URL规范
> 比如对员工 emp 增删改查的 url 规范  

| URL |Method| 功能
|:----:|:----:|:----:|
|/emps|GET|展示所有|
|/emp | GET | 去往添加页面|
|/emp | POST | 添加员工 |
|/emp/{id}| DELETE | 删除员工|
|/emp/{id}|GET| 去修改页面，回显|
|/emp| PUT | 提交修改，重定向到 list |  

- Add Emp: GET /emp + POST /emp
- Update Emp: GET /emp/{id} + PUT /emp

### 开发中技巧 
#### 1. 渲染模式需要准备的 html 页面
- `list.html` 展示页面  
&nbsp;&nbsp;
展示页面列出所有的信息，另外还有跳转到添加、删除记录、修改记录的超链接。
- `edit.html` 一页两用  
&nbsp;&nbsp;
为 add.html 和 update.html 合并版本。为了在 edit.html 一个文件中，提交时候体现差异，在渲染时候会选择性渲染 id 或其他唯一标志信息。具体细节接下。

#### 2. 由底向上实现一页两用
&nbsp;&nbsp;&nbsp;&nbsp;
<b>为 form 标签提供发送完整 RESTful 请求的能力：</b>
在 html 的 form 标签中，只支持 GET 和  POST 两种请求方式，为了能够 "发送" 出其他请求，可以和后端约定好添加某些字段来标记为其他请求方法。通常是在 form 表单为 POST 方式的前提下，添加一个隐藏的属性，属性名称为 `_method`，值为请求方式。
```html
<input type="hidden" name="_method" value="PUT"> 
```
但是这并不代表浏览器无法发送其他请求，如果是使用 Javascript 发送，就可以支持其他的方式。  
&nbsp;&nbsp;&nbsp;&nbsp;
<b>
更新数据和添加数据，在最接近数据库的 DAO 层，都是通过 save() 方法保存，若待更新数据已有主键(id)则更新数据，否则新建数据。  
&nbsp;&nbsp;&nbsp;&nbsp;
所以在通过控制器跳转到 edit.html 的时候，就要考虑是否分配 flag 来区分是新增还是更新，这个值也影响是否渲染隐藏域和其他页面显示信息。

