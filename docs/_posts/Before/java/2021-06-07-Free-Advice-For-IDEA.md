---
title: IDEA 使用前的最佳建议
categories: 
  - Java
description: 血泪成本得来的教训
keywords: IDEA
date: 2021-06-07 17:24:16
permalink: /pages/c6cc54/
sidebar: auto
tags: 
  - 
author: 
  name: Xueliang
  link: https://github.com/Human0722
---
> 先别急着写代码...  

从一开始学 Java 横冲直撞，先后用 brew 安装过 openjdk@8 和 openjdk@15, 也用二进制文件安装过 jdk@8，IDEA一编译，直接又给我安装了一个 jdk@?。导致后来运行项目的时候，出现莫名其妙的问题，耗时耗力。  
#### jdk8 足够
Java 目前的版本迭代太快了，但是很多项目用到的特性 `jdk8`足够支撑了。在自己电脑也就别用什么 `openjdk@?` 了，记得之前出过问题，换了 jdk8 就没问题。[jdk8 指 Oracle jdk]  
#### JAVA_HOME
只安装一个 jdk8 就足够了，如果不小心安装了多个 jdk,一定要配置好 `JAVA_HOME`, 因为有的程序是从 bash 的环境变量 $PATH 中寻找 Java 位置，有的却是从 JAVA_HOME 中寻找(如 Alibaba Nacos)。 如果你恰巧装了多个 jdk , 恰巧 JAVA_HOME 和环境变量里面的 Java 程序不指向同一个程序，那么莫名其妙的问题就会开始出现。
#### IDEA 环境设置推荐
不管你电脑上是否有 jdk 环境，IDEA 上来就给你整他自己的。当然，默认是最新的 jdk.  
IDEA设置分为两类,默认设置和项目设置。新建项目或直接导入的项目都会直接用默认设置，如果你配置好项目设置了，为了重建索引删了 `.idea` 文件夹，那么默认设置就会直接作用，莫名其妙的问题就会出现了。  
##### 默认设置之 jdk
<a href="https://www.cnblogs.com/zhangchengzi/p/9865232.html">设置默认JDK</a>  

##### 默认设置之 maven
<a href="https://www.cnblogs.com/zhangchengzi/p/9865100.html">设置默认 Maven</a>（推荐 Maven 3.5.+)

踩坑一周，好好的项目，依赖总是出问题 
(╥╯﹏╰╥) 
