---
title: 手动编写多模块项目的 pom 文件
categories: 
  - Java
description: pom file
keywords: pom
date: 2021-06-01 17:24:16
permalink: /pages/d9b9b1/
sidebar: auto
tags: 
  - 
author: 
  name: Xueliang
  link: https://github.com/Human0722
---
> 被 IDEA 的 Spring Initializr 整吐了..    

没错，被 Spring Initializr 生成的项目莫名其妙的报红整自闭了，还是自己新建 maven 项目手写 pom.xml吧 。  

父级 pom.xml 主要包括几个部分:  
  
    - gav 模块描述  
    - 子模块定义  
    - 打包方式  
    - 软件包版本变量定义  
    - 管理软件包版本  
    - 其他  

子模块 pom.xml 主要包括几个部分:   
  
    - 父模块声明  
    - gav 模块描述  
    - 依赖声明  

以下为参考 Demo。


### 父级 pom.xml    

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- 总项目描述 -->
    <groupId>org.atguigu.springcloud</groupId>
    <artifactId>mscloud</artifactId>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>cloud-provider-payment8001</module>
        <module>cloud-consumer-order80</module>
    </modules>
    <packaging>pom</packaging>

    <!--  定义版本变量  -->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <mysql.version>5.1.47</mysql.version>
    </properties>

    <!--
        --> 1. 下面的定义不会触发下载包
        --> 2. 子模块 pom 文件中使用到这些包的时候，不用再给出 version,统一子模块的包版本
    -->
    <dependencyManagement>
        <dependencies>
            <!--spring boot 2.2.2 依赖-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.2.2.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!-- spring cloud H版本 依赖-->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR1</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--spring cloud alibaba 2.1.0.RELEASE-->
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>2.1.0.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!-- mysql 版本用上 properties 定义的版本变量-->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <fork>true</fork>
                    <addResources>true</addResources>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>


```
### 子模块 pom  

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <!-- 非常重要， 类似于 java 继承-->     
    <parent>
        <artifactId>mscloud</artifactId>
        <groupId>org.atguigu.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-provider-payment8001</artifactId>

    <dependencies>
        <!--  https://mvnrepository.com/artifact/mysql/mysql-connector-java  -->
        <!-- 不用给出版本号，在父 pom 中定义好了,也可写出 version 强制使用新版本 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <!--  https://mvnrepository.com/artifact/org.projectlombok/lombok  -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!--  https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools  -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope >
            <optional>true</optional>
        </dependency>
    </dependencies>
</project>

```