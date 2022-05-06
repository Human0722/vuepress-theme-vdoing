---
title: IDEA 打包WEB应用后无法运行问题
categories: 
  - Java
description: null
keywords: idea, javaweb
date: 2021-04-11 17:24:16
permalink: /pages/3290a3/
sidebar: auto
tags: 
  - 
author: 
  name: Xueliang
  link: https://github.com/Human0722
---
> 

### 部署时默认不打包 lib 
一般我们会在项目根目录建立 lib 放 jar 包，也通过 idea 设置作用到项目了。可以正常编译、部署。但是访问时候，会报 ClassNotFoundException 异常，因为发布的时候没有带上 lib。以下为默认的部署配置，可以看到在 WEB-INF 目录下并没有 lib 库。 解决: 在右侧的 ssm (Project Library) 上选择 put into /WEB-INF/lib(idea version: 2021)

![nolib](/images/spring/nolib.jpg)


### 忽略非java文件  
当新建好新的 web 工程的时候，目录结构如图。IDEA 打包时，会忽略 java 文件夹下所有非 ```.java``` 的文件。 编写 Mybatis 的映射文件和接口时候。如 EmpMapper.xml 和 EmpMapper.java. 要求两个文件名相同，所属包也相同才可相互绑定。但将 EmpMapper.xml 放入 java 下的包中，部署时默认被忽略，访问时就会报错 没有匹配的映射文件。解决: 在 Resource 资源文件夹中存放 xml 文件。 如 com.abc.EmpMapper.java, 对应的就是 Resource/com/abc/EmpMapper.xml文件。

![ssmproject](/images/spring/ssmproject.jpg)
