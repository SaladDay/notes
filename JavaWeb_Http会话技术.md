---
title: Http会话技术
date: 2021-5-15 11:29:04
tags:
categories: JavaWeb
cover:
---

### 会话

1. 概念：一次会话中可以包含多次请求和响应
2. 功能：在一次会话的多次请求内共享数据
3. 方式：
  - 服务器端会话技术：Session
  - 客户端会话技术：Cookies

### Cookies

1. 概念：客户端会话技术，将数据保存到客户端

2. 实现原理：（事实上就是基于HTTP协议完成的技术）

   - 服务端通过responseHeader中set-cookies返回数据给客户端
   - 客户端解析responseHeader并将cookies数据存储
   - 在下一次请求中，在requestHeader中携带Cookies传给服务器

3. 细节：

   1. cookies的生命周期

      1. 默认情况下存储在浏览器内存中，关闭即释放
      2. setMaxAge(int seconds)
         - 正数：将Cookies数据写到硬盘中，secounds秒后删除
         - 负数：默认
         - 零：给浏览器发送请求，让浏览器立马删除硬盘中的cookies

   2. cookies共享问题

      1. 通过setPath(String path)方法指定 cookies在服务器中多个项目的共享域
      2. 通过setDomain(String path)方法指定 cookies在不同服务器之间的共享域，若设置".baidu.com"，那么cookies就可以在new.baidu.com和tieba.baidu.com之间共享

      - 归根结底：其实就是服务器告诉浏览器我这个cookies可以给谁用，让浏览器去做这件事。

4. 特点：

   1. cookies存储的数据在客户端浏览器
   2. 浏览器对于单个cookies的大小有限制(4kb)以及对于同一一级域名下的持久化cookies的数量有限制（15个）

5. 作用

   一般用于在不登陆的状态下，完成服务器对客户端的身份识别

### Session:

1. 概念：服务器端会话技术，在一次会话的多次请求中共享数据，将数据保存在服务器端。在Http协议中存在HttpSession对象
2. 用法：可以使用Request.getSession()来获取HttpSession
3. 原理：
   1. 用户第一次访问，服务器在cookies中没有找到JSESSION值，会自动在内存中创建一个session
   2. 同时将sessionid通过responseHeader中set-cookies字段(JSESSIONID=4216D0601BEED80EA9176EB555F23168)发回去，浏览器自动解析并且把其存入cookies
   3. 下次浏览器访问的时候携带cookies，服务器即可从已有session中取用数据，保证一个用户一次会话--一个session
4. 细节
   1. 基于实现原理，如果我们要延长Session生命周期，我们可以将sessionid通过cookie返回，并且设置cookie的age
5. 特点
   1. session用于储存一次会话的多次请求数据，存在服务器端
   2. 由于session在服务器端为对象的形式存储，因此其可以存储任意类型，任意大小的数据
