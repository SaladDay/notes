---
title: Servlet_执行原理
date: 2021-06-07 13:24:43
tags:
categories: Java
cover:
---
### 类对象创建原理
1. 当服务器接收到浏览器的请求时，会解析其URL地址，获取访问的Servlet资源路径。
2. 通过虚拟路径，在对应的项目下查找web.xml文件，是否有对应的\<url-parrern>标签体内容。
3. 如果有，则通过其找到对应的\<servlet-class>对象。
4. 通过classLoader将其加载进内存（class.forname()），并且获得其Class对象
5. 通过其Class对象创建其实例（newInstance），并且由于其实现了Servlet接口，则其必然有接口中方法，因此可以直接调用其方法。

以上整个方法（加载字节码.创建对象...）都是由==web容器（框架）==实现的，实现原理的基本理论是reflect。

### Request && Response

tomcat在会自动创建Request和Response对象，将两个对象传递给serve方法。





