---
title: JSON解析器
date: 2020-12-21 13:15:51
tags:
categories: JSON
cover:
---

### JSON与Java对象互转

- 注意：json在前端(js)中的表现是一个对象，在用ajax接受的时候可以设置返回对象类型为json，将其从后端发送的字符串重新转为json

1. JSON转为Java对象
   1. 选择合适的解析器，以jackson为例
   2. 导入jackson的相关jar包
   3. 创建jackson核心对象 ObjectMapper
   4. 调用ObjectMapper的相关方法进行转化
      1. readValue(String json,class)



2. Java对象转为JSON
   1. 选择合适的解析器，以jackson为例
   2. 导入jackson的相关jar包
   3. 创建jackson核心对象 ObjectMapper
   4. 调用ObjectMapper的相关方法进行转化
      1. 方法
         - writeValue(参数1,obj):参数1：
           1. File:将obj对象转化为JSON字符串，并保存在对应的文件中
           2. Writer:将obj对象转化为JSON字符串，并将JSON字符串填充到字符输入流中
           3. OutputStream:将obj对象转化为JSON字符串，并将JSON字符串填充到字节输入流中
         - writeValueAsString(obj):将obj对象转化为JSON字符串，并返回
      2. 注解：
         1. @JsonIgnore:排除属性
         2. @JsonFormate:属性值格式化

