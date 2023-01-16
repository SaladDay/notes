---
title: redis
date: 2021-05-21 14:54:45
tags:
categories: SQL
cover:
---

### 数据类型以及命令行操作
1. redis的数据结构：
    redis存储的是：key,value格式的数据，其中key都是字符串，value有5种不同的数据结构

    1. 字符串类型 string

    2. 哈希类型 hash ： map格式  

    3. 列表类型 list ： linkedlist格式。支持重复元素

    4. 集合类型 set  ： 不允许重复元素

    5. 有序集合类型 sortedset：不允许重复元素，且元素有顺序

2. 命令行操作

    1. 字符串类型 String

        ```bash
        1. 存储： set key value
        2. 获取： get key
        3. 删除： del key
        ```

    2. 哈希类型 Hash

       ```bash
       1.存储：hset key field value
       2.获取：
       	- hget key field
       	- hgetall key
       3.删除：hdel key field
       ```

    3. 列表类型 List

    ```bash
    1.添加：
    	- lpush key value
    	- rpush key value
    2.获取：lrange key start end
    	- 一般使用 lrange key 0 -1 来获取全部数据
    3.删除：
    	- lpop key
    	- rpop key
    
    ```

    4. 集合类型：set：不允许重复元素

       ```bash
       1.存储：sadd key value
       2.获取：smembers key
       3.删除：srem key value
       ```

    5. 有序集合类型：sortedset：不允许重复元素，且元素有顺序； 每个元素都会关联一个double类型的score。redis正是通过分数来为集合中的成员进行从小到大的排序。

    ```bash
    1.存储：zadd key score value
    2.获取：zrange key start end 
    3.删除：zrem key value
    ```

    6. 通用命令

    ```bash
    1.keys [regex] :查找键
    	- keys * :查找所有的键
    2.type key: 获得键对应的value类型
    3.del key: 删除指定的key value(键值对) 
    ```

    

