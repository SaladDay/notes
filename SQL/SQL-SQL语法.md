---
title: SQL_SQL语法
date: 2020-12-06 12:40:10
tags:
categories: SQL
cover: 
---

## SQL分类
- DDL（Data Definition Language）数据定义语言。用来定义数据库对象。
- DML（Data Manipulation Language）数据操作语言。用来对数据库中表的数据进行增删改。
- DQL（Data Query Language）数据查询语言。用来查询数据库中表的数据（记录）。
- DCL（Data Control Language）数据控制语言。用来定义数据库的访问权限和安全级别，以及创建用户。








## DDL:操作数据库、表

	1. 操作数据库：CRUD
		1. C(Create):创建
			* 创建数据库：
				* create database 数据库名称;
			* 创建数据库，判断不存在，再创建：
				* create database if not exists 数据库名称;
			* 创建数据库，并指定字符集
				* create database 数据库名称 character set 字符集名;
	
			* 练习： 创建db4数据库，判断是否存在，并制定字符集为gbk
				* create database if not exists db4 character set gbk;
		2. R(Retrieve)：查询
			* 查询所有数据库的名称:
				* show databases;
			* 查询某个数据库的字符集:查询某个数据库的创建语句
				* show create database 数据库名称;
		3. U(Update):修改
			* 修改数据库的字符集
				* alter database 数据库名称 character set 字符集名称;
		4. D(Delete):删除
			* 删除数据库
				* drop database 数据库名称;
			* 判断数据库存在，存在再删除
				* drop database if exists 数据库名称;
		5. 使用数据库
			* 查询当前正在使用的数据库名称
				* select database();
			* 使用数据库
				* use 数据库名称;


```sql
2. 操作表
	1. C(Create):创建
		1. 语法：
			create table 表名(
				列名1 数据类型1,
				列名2 数据类型2,
				....
				列名n 数据类型n
			);
			* 注意：最后一列，不需要加逗号（,）
			* 数据库类型：
				1. int：整数类型
					* age int,
				2. double:小数类型
					* score double(5,2)
				3. date:日期，只包含年月日，yyyy-MM-dd
				4. datetime:日期，包含年月日时分秒	 yyyy-MM-dd HH:mm:ss
				5. timestamp:时间错类型	包含年月日时分秒	 yyyy-MM-dd HH:mm:ss	
					* 如果将来不给这个字段赋值，或赋值为null，则默认使用当前的系统时间，来自动赋值

				6. varchar：字符串
					* name varchar(20):姓名最大20个字符
					* zhangsan 8个字符  张三 2个字符
```


			* 创建表
				create table student(
					id int,
					name varchar(32),
					age int ,
					score double(4,1),
					birthday date,
					insert_time timestamp
				);
			* 复制表：
				* create table 表名 like 被复制的表名;	  	
		2. R(Retrieve)：查询
			* 查询某个数据库中所有的表名称
				* show tables;
			* 查询表结构
				* desc 表名;
		3. U(Update):修改
			1. 修改表名
				alter table 表名 rename to 新的表名;
			2. 修改表的字符集
				alter table 表名 character set 字符集名称;
			3. 添加一列
				alter table 表名 add 列名 数据类型;
			4. 修改列名称 类型
				alter table 表名 change 列名 新列名 新数据类型;
				alter table 表名 modify 列名 新数据类型;
			5. 删除列
				alter table 表名 drop 列名;
		4. D(Delete):删除
			* drop table 表名;
			* drop table  if exists 表名 ;





## DML（modify）：增删改表中数据

	1. 添加数据：
		* 语法：
			* insert into 表名(列名1,列名2,...列名n) values(值1,值2,...值n);
		* 注意：
			1. 列名和值要一一对应。
			2. 如果表名后，不定义列名，则默认给所有列添加值
				insert into 表名 values(值1,值2,...值n);
			3. 除了数字类型，其他类型需要使用引号(单双都可以)引起来
	2. 删除数据：
		* 语法：
			* delete from 表名 [where 条件]
		* 注意：
			1. 如果不加条件，则删除表中所有记录。
			2. 如果要删除所有记录
				1. delete from 表名; -- 不推荐使用。有多少条记录就会执行多少次删除操作
				2. TRUNCATE TABLE 表名; -- 推荐使用，效率更高 先删除表，然后再创建一张一样的表。
	3. 修改数据：
		* 语法：
			* update 表名 set 列名1 = 值1, 列名2 = 值2,... [where 条件];
	
		* 注意：
			1. 如果不加任何条件，则会将表中所有记录全部修改。



## DQL：查询表中的记录
	* select * from 表名;
	
	1. 语法：
		select
			字段列表
		from
			表名列表
		where
			条件列表
		group by
			分组字段
		having
			分组之后的条件
		order by
			排序
		limit
			分页限定


	2. 基础查询
		1. 多个字段的查询
			select 字段名1，字段名2... from 表名；
			* 注意：
				* 如果查询所有字段，则可以使用*来替代字段列表。
		2. 去除重复：
			* distinct
		3. 计算列
			* 一般可以使用四则运算计算一些列的值。（一般只会进行数值型的计算）
			* ifnull(表达式1,表达式2)：null参与的运算，计算结果都为null
				* 表达式1：哪个字段需要判断是否为null
				* 如果该字段为null后的替换值。
		4. 起别名：
			* as：as也可以省略


	3. 条件查询
		1. where子句后跟条件
		2. 运算符
			* > 、< 、<= 、>= 、= 、<>
			* BETWEEN...AND  
			* IN( 集合) 
			* LIKE：模糊查询
				* 占位符：
					* _:单个任意字符
					* %：多个任意字符
			* IS NULL  
			* and  或 &&
			* or  或 || 
			* not  或 !

```sql
1. 排序查询
	* 语法：order by 子句
		* order by 排序字段1 排序方式1 ，  排序字段2 排序方式2...

	* 排序方式：
		* ASC：升序，默认的。
		* DESC：降序。

	* 注意：
		* 如果有多个排序条件，则当前边的条件值一样时，才会判断第二条件。
```

```sql
2. 聚合函数：将一列数据作为一个整体，进行纵向的计算。
	1. count：计算个数
		1. 一般选择非空的列：主键
		2. count(*)
	2. max：计算最大值
	3. min：计算最小值
	4. sum：计算和
	5. avg：计算平均值
	* 注意：聚合函数的计算，排除null值。
		解决方案：
			1. 选择不包含非空的列进行计算
			2. IFNULL函数
```

```sql
3. 分组查询:
	1. 语法：group by 分组字段；
	2. 注意：
		1. 分组了以后个性的东西就消失了，仅仅具有共性的东西，因此有2；
		2. 分组之后查询的字段：==分组字段、聚合函数==
		3. where 和 having 的区别？
			1. where 在分组之前进行限定，如果不满足条件，则不参与分组。				having在分组之后进行限定，如果不满足结果，则不会被查询出来
			2. where 后不可以跟聚合函数（因为此时是对个体的判断），having可			以进行聚合函数的判断。（having是对团队的判断）

```



## DCL：管理用户并对其授权

```sql
1.管理用户
	1. 查询：操作mysql数据库中的user数据表
	2. 创建：CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
	3. 删除：DROP USER '用户名'@'主机名';
	4. 修改密码： SET PASSWORD FOR '用户名'@'主机名' = PASSWORD('新密码');

2.授权
	1. 查询权限：SHOW GRANTS FOR '用户名'@'主机名';
	2. 授予权限：grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
		（*.*表示所有数据库中的所有数据表）
	3. 撤销权限：revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
```



## 约束

* 概念： 对表中的数据进行限定，保证数据的正确性、有效性和完整性。	
* 分类：
	1. 主键约束：primary key
	2. 非空约束：not null
	3. 唯一约束：unique
	4. 外键约束：foreign key





```sql
* 非空约束：not null，某一列的值不能为null
	1. 创建表时添加约束
		CREATE TABLE stu(
			id INT,
			NAME VARCHAR(20) NOT NULL -- name为非空
		);
	2. 创建表完后，添加非空约束
		ALTER TABLE stu MODIFY NAME VARCHAR(20) NOT NULL;

	3. 删除name的非空约束
		ALTER TABLE stu MODIFY NAME VARCHAR(20);
```

```sql
* 唯一约束：unique，某一列的值不能重复
	1. 注意：
		* 唯一约束可以有NULL值，但是只能有一条记录为null
	2. 在创建表时，添加唯一约束
		CREATE TABLE stu(
			id INT,
			phone_number VARCHAR(20) UNIQUE -- 手机号
		);
	3. 删除唯一约束
		ALTER TABLE stu DROP INDEX phone_number;
	4. 在表创建完后，添加唯一约束
		ALTER TABLE stu MODIFY phone_number VARCHAR(20) UNIQUE;
```



```sql
* 主键约束：primary key。
	1. 注意：
		1. 含义：非空且唯一
		2. 一张表只能有一个字段为主键
		3. 主键就是表中记录的唯一标识

	2. 在创建表时，添加主键约束
		create table stu(
			id int primary key,-- 给id添加主键约束
			name varchar(20)
		);

	3. 删除主键
		-- 错误 alter table stu modify id int ;
		ALTER TABLE stu DROP PRIMARY KEY;

	4. 创建完表后，添加主键
		ALTER TABLE stu MODIFY id INT PRIMARY KEY;

	5. 自动增长：
		1.  概念：如果某一列是数值类型的，使用 auto_increment 可以来完成值得自动增长

		2. 在创建表时，添加主键约束，并且完成主键自增长
		create table stu(
			id int primary key auto_increment,-- 给id添加主键约束
			name varchar(20)
		);
		3. 删除自动增长
		ALTER TABLE stu MODIFY id INT;
		4. 添加自动增长
		ALTER TABLE stu MODIFY id INT AUTO_INCREMENT;
```





```sql
* 外键约束：foreign key,让表于表产生关系，从而保证数据的正确性。
	1. 在创建表时，可以添加外键
		* 语法：
			create table 表名(
				....
				外键列
				constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称)
			);

	2. 删除外键
		ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;

	3. 创建表之后，添加外键
		ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称);
```

```sql
	4. 级联操作
		1. 添加级联操作
			语法：ALTER TABLE 表名 ADD CONSTRAINT 外键名称 
					FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列					名称) ON UPDATE CASCADE ON DELETE CASCADE  ;
		2. 分类：
			1. 级联更新：ON UPDATE CASCADE 
			2. 级联删除：ON DELETE CAS CADE 
```

## 

