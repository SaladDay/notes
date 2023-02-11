---
title: JDBC
date: 2020-12-08 17:15:05
tags:
categories: Java
cover:
---

- 全称：Java Database connect
- 本质：SUM公司定义的一套操作关系型数据库的接口。各个数据库厂商需要去实现接口，构建实现类，并且打包提供jar包。Coder可以使用这套接口（JDBC）编程，真正执行的代码是Jar包中的实现类。

对象：

 1. DriverManager：驱动管理器对象，定义在java.sql中

    - 注册驱动（告诉程序该使用哪一个驱动jar包来操作数据库\操作的是什么数据库）

      ```java
      1.
      DriverManager.registerDriver(new com.mysql.jdbc.Driver());
      2.
      Class.forname("com.mysql.jdbc.Driver");
      //这句代码加载了Driver类，而此类中有静态代码块，其中便是1代码加上trycatch语句
      
      ```

    - 获取数据库连接：

      - static Connection getConnection();

 2. Connection:数据库连接对象

     1. 获取Statement\PrepareStatement对象
     2. 管理事务

 3. Statement: 执行SQL语句对象

     1. execute():可执行任意的sql语句，不常用
     2. int executeUpdate():执行DML和DDL语句
     3. ResultSet executeQuery():执行DQL语句

 4. ResultSet：结果集对象

    - 游标驱动，相当于一个迭代器

 5. PreparedStatement ：预编译statement

      1. 防止SQL注入：将用户输入的 ' 进行转义，不让用户输入
      2. 提前编译，提高批量执行效率


### 数据库连接池

-  概念：其实就是一个容器，存放数据库连接Connection对象的容器
-  好处：节约资源、用户获取高效
-  实现：Java提供标准接口DataSource，内部有方法getConnection。厂商去实现此接口，目前市面上有技术：（C3P0,Druid）

### Spring JDBC

- Spring框架对JDBC的简单封装，提供了一个JDBCTemplate对象简化JDBC的开发流程,JDBCTemplate在创建的时候需要提供一个连接池对象。

- 常用操作

  - update() : DML

  - queryForMap() : 查找一条记录，并将其以map的形式返回

  - queryForList() : 查找多条记录，每条记录是一个map，返回一个List

  - queryForObject() : 查询结果，将结果封装为对象。一般用于聚合函数。

  - query() : 查询结果，将查找到的对象封装成javaBean，返回其类对象的List。参数：rowMapper，将行对应javaBean中的属性

    
