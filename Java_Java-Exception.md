---
title: Java_Exception
date: 2020-12-05 12:38:08
tags:
categories: Java
cover:
---

![image-20221205123847696](https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221205123847696.png)

- throws: 写在方法定义处，表示声明一个异常。告诉调用者，使用本方法可能会有哪些异常。
- throw：写在方法内，结束方法（类似于return）。手动抛出异常，交给调用者。

### 自定义异常：

意义：为了让控制台的报错信息更加见名知意。（就是为了那个名字）

继承 RuntimeException // Exception

