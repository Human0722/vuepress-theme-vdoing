---
title: SpringMVC 文件下载与上传
categories: 
  - Java
description: download && upload code
keywords: SpringMVC, JavaWeb
date: 2021-03-19 17:24:16
permalink: /pages/04b5e2/
sidebar: auto
tags: 
  - 
author: 
  name: Xueliang
  link: https://github.com/Human0722
---
> SpringMVC 上传&&下载的代码 

### 1. 文件下载 
```java
// @Controller
@RequestMapping("/download", method = RequestMethod.GET)
public MultipartEntry<byte[]> download(HttpSession session) throws IOException{
    // Get file store location on server
    String realPath = session.getServletContext().getRealPath("img");
    String finalPath = realPath + File.separator + "2.jgp";
    InputStream is = new FileInputStream(finalPath);
    // available(): 获取输入流所读取文件的最大字节数
    byte[] b = new byte[is.available()];
    is.read(b);
    // 设置响应头
    HttpHeaders headers = new HttpHeaders();
    headers.add("Content-Disposition", "attachment;filename=zzz.jgp");
    // 设置响应状态
    HttpStatus statusCode = HttpStatus.OK;
    ResponseEntity<byte[]> entity = new ResponseEntity<byte[]>(b, headers, statusCode);

    return entity;
}
```  

### 2. 文件上传  
> 从表单开始，到 SpringMVC 的代码设置
#### 2.1 html表单  

```html
<form action="/up" method="post" enctype="multipart/form-data">
    <input type="file" name="uploadFile">
    <input type="submit" name="upload">
</form>
```  

#### 2.2 控制器处理方法  
```java
// 原生方法
@RequestMapping(value = "up", method = RequestMethod.POST)
public String up(String desc, MultipartFile uploadFile, HttpSession session)  
throws IOException{
    // get uploadFile filename
    String fileName = uploadFile.getOriginalFilename();
    String path = session.getServletContext().getRealPath("photo") + File.separator + fileName;
    // get inputStream
    InputStream is = uploadFile.getInputStream();
    //get outputStream
    File file = new File(path);
    OutputStream os = new FileOutputStream(file);
    //write
    byte[] b = new byte[1024];
    while((i = is.read(b)) != -1) {
        os.write(b, 0, i);
    }
    os.close();
    is.close();
    return "success";
}

// 封装类方法
    @RequestMapping(value="/up", method=RequestMethod.POST)
	public String up(String desc, MultipartFile uploadFile, HttpSession session) throws IOException {
		//获取上传文件的名称
		String fileName = uploadFile.getOriginalFilename();
		String finalFileName = UUID.randomUUID() + fileName.substring(fileName.lastIndexOf("."));
		String path = session.getServletContext().getRealPath("photo") + File.separator + finalFileName;
		File file = new File(path);
		uploadFile.transferTo(file);
		return "success";
	}
```  
#### 2.3 配置 multipartResolver 
> multipartResolver 是用来处理上传文件的,前置条件: 导入 commmons.io、 commons.fileupload 包。  

<b>在 SpringMVC 的配置文件中新增 </b>  
```xml
<bean id="multipartResolver" class="xxx.xxx.xxx.CommonsMultipartResolver">
    <property name="defaultEncoding" value="UTF-8" />
    <property name="maxUploadSize" value="10240000">
</bean>
```
#### 2.4 Tips  
- 上传文件保存位置的 photo 文件夹，要放在 WebContent 目录下，有时候空目录部署时会被忽略掉，最好在目录中放个文件。
- 上传文件处理函数的参数， Multipart uploadFile 中的属性名要和表单中 file 类型的 input 的 name 属性值一致，或者用 @RequestParam() 的方式来绑定。