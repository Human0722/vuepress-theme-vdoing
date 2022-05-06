---
title: Nginx 搭建域名访问微服务
categories: 
  - Java
description: Nginx 搭建域名访问微服务
keywords: springcloud, gateway, nginx
date: 2021-07-27 17:24:16
permalink: /pages/45212c/
sidebar: auto
tags: 
  - 
author: 
  name: Xueliang
  link: https://github.com/Human0722
---
> 部署微服务  

如下图，用户通过域名访问到 Nginx，Nginx 反向代理到 SpringCloud Gateway 网关微服务，再由网关根据具体规则转发到不同的微服务。

```txt


                                                                  ┌────────────────┐
                                                                  │                │
                                                        ┌─────────►  MicroService  │
                                                        │         │                │
                    ┌────────┐       ┌──────────────┐   │         └────────────────┘
                    │        │       │              │   │                 .
     ┌──────┐       │        │       │  SpringCloud ├───┤                 .
     │ User ├──────►│ Nginx  ├───────►    Gateway   │   │                 .
     └──────┘       │        │       │              │   │         ┌────────────────┐
                    └────────┘       └──────────────┘   │         │                │
                                                        └─────────►  MicroService  │
                                                                  │                │
                                                                  └────────────────┘



```

虽然 SpringCloud Gateway 可以直接在80端口提供服务，但是 Nginx 可以高效地处理静态资源，两者功能有重叠部分，各自负责优势部分。    

#### 1. 配置域名    

> User ---> Nginx

``` > echo "yunmall.com YOUR_NGINX_SERVER_IP" >> /etc/hosts```   



#### 2.Nginx 反向代理 Gateway    

> Nginx -> SpringCloud Gateway

Nginx 反向代理 SpringCloud Gateway, 先要在 nginx 总配置文件 nginx.conf 中添加 ***upstream*** 配置块配置上游服务器。然后在虚拟主机配置文件中，将所有匹配的域名请求转发到网关。  

<font color="red">Nginx 转发请求时会丢失域名信息,网关断言会失效，所以要设置转发域名。</font>详见`yunmall.conf@Line10` ,其中 $host 代表 `server_name` 配置项的值。

***nginx.conf***   

```
....
http {
	....
	upstream yunmall {
		server 192.168.56.1:8888;  #YOUR_GATEWAY_SERVICE_IP
	}
  include /etc/nginx/conf.d/*.conf;
}
```

***yunmall.conf***  

```
server {
    listen       80;
    server_name  gulimall.com;

    #charset koi8-r;
    #access_log  /var/log/nginx/log/host.access.log  main;

    location / {
    # Nginx 转发请求时，以 IP 方式而非域名方式转发给网关，所以需要配置设置头信息。
      proxy_set_header Host $host;
      proxy_pass http://gulimall;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```



#### 3. 网关转发请求到微服务    

> SpringCloud Gateway ---> MicroService

***网关微服务配置文件： application.yml*** 

```yaml
spring:
	cloud:
		gateway:
			routes:
				- id: index_route
          uri: lb://yunmall-product
          predicates:
            - Host=yunmall.com,**.yunmall.com
```



#### 4. Nginx 自行处理静态请求  

> SpringBoot 应用同样可以处理静态资源，但性能远不及 Nginx  

**yunmall.conf**

```
location /static/ {
  root /user/share/nginx/html;
}
```

将所有的静态请求改为 `/static/` 开头，Nginx 就会自动拦截并处理，这是 Nginx 的动静分离。  



#### 5. Conclusion 

记录简易微服务架构的部署，包含 Nginx 的动静分离和反向代理两个功能，也可在 `upstream` 中定义多个网关，实现负载均衡。

