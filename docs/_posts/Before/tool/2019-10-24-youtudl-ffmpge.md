---
title: Youtube视频下载转音频
categories: 
  - Tool
description: null
keywords: 小工具
date: 2019-10-24 17:24:16
permalink: /pages/af27af/
sidebar: auto
tags: 
  - 
author: 
  name: Xueliang
  link: https://github.com/Human0722
---

> 下载 Youtube 视频音乐

环境准备：
- 梯子
- youtube-dl
- ffmpeg

### 查看梯子提供的代理端口
看出本地 ```socks5``` 代理端口是： 1086。
![proxy](/images/tool/socksport.jpg)
### 列出各种清晰度
记住选择的清晰度前面的数字。 ```-F``` 表示列出所有清晰度。 ```--proxy socks5://127.0.0.1:1086``` 命令指定从代理下载。
![list](/images/tool/youtube-list.jpg)
### 下载这个视频
用 ```-f + 数字``` 下载上面选择的清晰度。
![list](/images/tool/youtube-download.jpg)  

### 下载整个列表

下载视频列表有两种方式，分别是:  

```shell
youtube-dl -i -f mp4 --yes-playlist 'https://www.youtube.com/watch?v=7Vy8970q0Xc&list=PLwJ2VKmefmxpUJEGB1ff6yUZ5Zd7Gegn2'
```



```shell
youtube-dl -i PLRMOX8QaZK8wYHSTn0OmjvQqzZsUug-Gu
```



### 提取音频编码

``` ffmpeg``` 工具 ```-i``` 指定输入文件, ```-f``` 指定输出格式, 后跟文件名。
![convert](/images/tool/youtube-convert.jpg)

### 存到网易云音乐云盘
网易云盘``` 40G```足够放很多歌了。
![music](/images/tool/youtube-music.jpg)