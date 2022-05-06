---
title: SpringMVC HelloWorld Application in STS4
categories: 
  - Java
description: 记录一下在 Spring Tool Suite4 中配置 SpringMVC项目
keywords: SpringMVC, JavaWeb
date: 2021-03-19 17:24:16
permalink: /pages/781ce8/
sidebar: auto
tags: 
  - 
author: 
  name: Xueliang
  link: https://github.com/Human0722
---
> Hello, SpringMVC ~   

``` json
jdk:    1.8
Tomcat: 7
Spring: 4.0
Spring Tool Suite:  4
```


### 讲在前面   
SpringMVC 是对 Servlet 的封装。 Servlet 是一种约束，是接口。只要我们编写的 Java 程序实现了这个接口, 程序就叫做 Servlet 程序。 和 Tomcat 的关系的是， Tomcat 调用我们编写的 Servlet 程序, 同时传进来一些用户访问信息的封装类。因为 Servlet 程序受到 Servlet 接口的约束， Tomcat 就能保证调用 Servlet 程序一定可用。 原始的 Servlet 编程每个 url 都要编写对应的 servlet 配置块,来实现 url 和 Servlet 功能的映射，效率低下。而 SpringMVC 中的 DispatcherServlet 实现了 Servlet接口，保证能够被 Tomcat(或 Servlet 容器)调用，且这个类接管了所有的 url 地址，具体细分的地址和对应的功能实现用高效的注解绑定，绑定抛弃了繁琐的 xml 注解，先是将功能类用 Spring 的扫描器加载到容器中，然后再给方法加上注解，来标注这个方法是为了实现哪个 url 功能的而实现绑定。


### 1.新建一个动态 Web 工程  
![new Project](/images/spring/newProject.jpg)
![create Project](/images/spring/createProject.jpg)
注意：如果生成的模板中，在 ```ProjectName/WebContext/WEB-INF/``` 下没有 web.xml, 则按下图操作点击后，则会自动生成 web.xml .
![createWebXml](/images/spring/createWebXml.jpg)  

导入 SpringMVC 所需要的包:(在 WEB-INF 下新建lib 包，然后选中所有包，add build path)

![packages](/images/spring/packages.jpg) 


### 2.将所有的 url 请求分配给 DispatcherServlet 处理  
#### 2.1 在 Tomcat 中配置 DispatcherServlet

在 web.xml 中追加一下内容:  
```xml
<web-app>
  ...
  <servlet>   <!-- 注册一个 Servlet -->
    <servlet-name>springmvc</servlet-name>  <!-- Servlet 名称-->
    <!--实现了 Servlet 的具体类名-->
	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class> 
  </servlet>
  <servlet-mapping>  <!-- 注册 Servlet 和 url 的映射关系-->
	<servlet-name>springmvc</servlet-name>
	<url-pattern>/</url-pattern> <!-- 所有的 url -->
  </servlet-mapping>
</web-app>
```
#### 2.2 为 SpringMVC 编写配置文件  
    DispatcherServlet 的配置文件，要求是放在 `WEB-INF` 下，名称格式为: `<servlet-name>-servlet.xml`(以 New File [> Other]> Spring Bean Configuration File新建,如果没有这个选项，需要从商店安装 Spring Tools 3 Add-On). 
```xml
<beans ....>
  	<!--  Spring的自动扫描组件，将控制器扫描装载到容器中 -->
	<context:component-scan base-package="com.atguigu.springmvc"></context:component-scan>
	
	<!-- 将方法返回的字符串映射成视图的规则 -->
	<bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/view/"></property>	
		<property name="suffix" value=".jsp"></property>
	</bean>
</beans>
```  
#### 2.3 创建处理方法，并映射 url

按照上文的 base-package 创建包， 创建 HelloController 类。其中 `@Controller` 注解为了容器能够扫描到这个类,这是类被使用的前提。 `@RequestMapping()` 注册了哪个 url 能直接访问这个方法。
![mapping](/images/spring/mapping.jpg)

### 3. 处理视图层
 1. 在2.3 中返回的字符串 "success", 将被 2.2 中的配置渲染成 `/WEB-INF/view/success.jsp`. 我们的项目中并没有这个文件，手动创建。
 2. 在 `/WEB-INF/` 下创建 index.jsp, 生成跳转到 `/hello` 的超链接.  
![dir](/images/spring/dir.jpg)

### 4. 在 STS4 中配置 Tomcat 服务器。  
#### 4.1 点击 Servers 下的添加服务器

 ![createTomcat](/images/spring/createTomcat.jpg)  

#### 4.2 选择自己机器的环境
 ![createWebXml](/images/spring/tomcatInfo.jpg)  

#### 4.3 Run !
 ![deployment   ](/images/spring/deployment.jpg)  
#### 4.4 访问
 ![browser](/images/spring/browser.jpg)  