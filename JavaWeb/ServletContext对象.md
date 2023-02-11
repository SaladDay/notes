---
title: ServletContext对象
date: 2020-12-13 16:26:59
tags:
categories: JavaWeb
cover:
---

1. 概念：(环境对象)代表整个web应用，可以和程序的容器(服务器Tomcat)来通信

2. 获取：

  3. 通过request对象获取

  4. 通过HttpServlet获取

3. 功能：
   1. 获取MINE类型
      - MINE类型：在互联网通讯的过程中定义的一种文件数据类型；例： text/html
      
   2. 域对象：共享数据
      - 域范围：所有用户，所有请求。（对象的生命周期很长，从服务器开始到服务器关闭）
      
   3. 获取文件在服务器下的真实路径
      1. getRealPath("/b.txt")  -->事实上就是项目根目录的b.txt，也就是web目录下的
      2. getRealPath(/WEB-INF/class/a.txt)       --->src下的东西到最后会被放到web/WEB-INF下的class目录下
         - 使用classLoader可以获取src下的资源路径也就是因为 src下的东西会被classLoader一起放到class目录下
      
      - 本质：获取Web应用在服务器中的根路径(也就是web文件夹的路径)，然后再和用户提供的参数进行拼接。