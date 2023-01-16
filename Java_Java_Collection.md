---
title: Java_Collection
date: 2020-12-02 00:25:50
tags:
categories: Java
cover:
---

![image-20221204164454529](https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221204164454529.png)

## Set集合的实现类特点

- HashSet：无序、不重复、无索引
- LinkedHashSet：有序、不重复、无索引
- TreeSet：可排序、不重复、无索引

### HashSet底层原理

- JDK8以前，数组+链表
- JDK8以后，数组+链表+红黑树（链表长度超过8，数组长度超过64的时候，此链表自动转化为红黑树）

tips:如果集合中储存的是自定义对象，则必须要重写`hashcode`和`equals`方法；(保证去重)

### LinkHashSet底层原理

底层数据结构依然是哈希表，只是每个元素又额外的多了一个双向链表的机制记录存储的顺序。

add的时候创建双向列表，遍历的时候去遍历双向链表。

以此来保证有序。

### TreeSet底层原理

底层是红黑树。增删改查性能较好。

> 红黑树：AVL树是带有平衡条件的二叉查找树，一般使用平衡因子差值判断是否平衡并且通过旋转来实现平衡，然而旋转操作的时间代价是很大的，我们不希望重复的去旋转。因此就有了红黑树，红黑树是一种BST但是不是AVL，其是遵循红黑平衡的BST，通过自己的一套规则减少了旋转的次数。

需要指定排序规则或者引入比较器（在构造函数时传入）；compareTo函数，返回值负数：存左边；返回值正数：存右边；返回值为0，认为元素已存在，舍弃。

## 使用场景

1. 如果想要集合中的元素可以重复
   - 用ArrayList
2. 如果想要集合中的元素可以重复，并且增删操作明显多于查询操作
   - 用LinkedList
3. 如果想对集合进行去重
   - 用HashSet
4. 如果想对集合进行去重，并且保证存取顺序
   - 用LinkedHashSet
5. 如果想对集合中的元素进行排序
   - 用TreeSet

---

## 双列集合

相较于单列集合，其实就是原本是存一个object进去，现在存了Entry<object,object>进去了。集合的所有特性都取决于键，值在集合中只是一个附加的数据。

![image-20221204210719453](https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221204210719453.png)

Entry在Map中的地位就相当于Node在List中的地位，是一个基本的数据单元。

### HashMap的底层原理

利用键计算出索引，将Entry加入哈希表。

倚赖hashCode()和equals()方法保证键的唯一。（自定义对象如果没有重写hashcode，则采用的是地址值的方式来生成hashcode）

如果要加入索引处的Entry的键与要加入的Entry的键相同，则覆盖（非舍弃）。

### LinkedHashMap的底层原理

同LinkedHashSet

### TreeMap的底层原理

底层和TreeSet一致都是红黑树结构。

所有的特性都是有键决定的。

可排序：对键进行排序。（Comparable接口，创建TreeMap时传递Comparator比较器对象）







