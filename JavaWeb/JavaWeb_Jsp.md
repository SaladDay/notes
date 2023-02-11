---
title: Jsp
date: 2021-05-16 13:14:29
tags:
categories: JavaWeb
cover:
---

## JSP

1. 概念：JavaServer Pages，可以通过特殊的标签将Java代码放入HTML中以实现动态内容开发。事实上就是一个系统自动生成的Servlet对象，会将写的java代码和网页都放在service()函数中
2. 指令：
   - 用于配置JSP页面
   - 分类
     1. page	：配置JSP页面
        - contentType:等同于response.setContentType()
          1. 告诉浏览器我的MIME类型以及字符集
          2. 设置我当前的字符集
     2. include ：导入页面的资源文件（相当于把别的页面复制过来）
     3. taglib    ：导入标签资源（相当于标签包）
        - 假如我需要使用JSP标准标签库，那么我就需要引用JSTL。(JSP  standarded tag library)

3. 内置对象：
   - 在jsp页面中不需要创建的对象。原因：在JSP转化为Servlet后，在service函数中已经被创建的对象。

| 变量名      | 真实类型            | 作用                                         |
| ----------- | ------------------- | -------------------------------------------- |
| pageContext | PageContext         | 当前页面共享数据，还可以获取其他八个内置对象 |
| request     | HttpServletRequest  | 一次请求访问的多个资源(转发)                 |
| session     | HttpSession         | 一次会话的多个请求                           |
| application | ServletContext      | 从服务器开启到关闭，多个用户，多个会话       |
| response    | HttpServletResponse | 响应对象                                     |
| page        | Object（Servlet）   | 当前页面Servlet ,this                        |
| out         | JspWriter           | 输出对象，相当于Writer                       |
| config      | ServletConfig       | Servlet配置对象                              |
| exception   | Throwable           | 异常对象                                     |

## EL表达式

1. 引言:根据MVC开发模式，我们应该尽量将少的JAVA代码放入View(即Jsp)当中。
2. 功能：
   1. 运算（简单的运算）
   2. 获取数据（主要功能）
      - 只能从域对象中获取数据
        1. 如果在域中存储的是简单类型：可以直接使用\${域名称.键名}获取
        2. 如果在域中存储的是对象：则需要通过对象的property(属性)来获取：\${域名称.键名.属性名}，本质上是调用了java类的getter方法。如果不加属性值，事实上是调用了toString方法。
        3. 如果在域中存储的是List集合：${域名称.键名[索引]}

## JSTL

1. 概念：JSP standarded tag library（JSP标准标签库）
2. 作用：用于简化和替换jsp页面上的java代码









