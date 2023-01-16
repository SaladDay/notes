---
title: Java中对象的内存图解
date: 2020-05-25 09:43:09
tags:
categories: Java
cover:
---
示例代码：
![img](https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/20180330234239226)


1. 当程序运行的时候，会将编译好的class文件进行加载，此时方法区中会有==class文件区==；class区内有成员属性、成员方法（实际上是一个虚方法表，会覆盖重载父类的函数）以及父类指针，但没有静态的属性和方法，在虚拟机加载该class文件的时，会自动把他所有静态的属性和方法分配到静态区（静态区也是在方法区内）。

​      此时实际上静态的内容已经加载完了，如果有显示初始化此时会初始化。

<img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221125095619415.png" alt="image-20221125095619415" style="zoom:50%;" />

2. 虚拟机自动在静态区寻找main方法，不需要创造对象，可以根据类名调用。

3. 执行```Person p1 = new Person("邓丽君",16,中国)；```

   3.1. 首先在栈中创建p1，可以用来放其地址

   3.2. new Person在堆内存中创建一个空间，假设其地址为0X0001，他也代表this。同时，虚拟机会自动根据class文件中的虚方法表，在方法区中生成一个Person的方法表，假设其地址为0x02；

   3.3. 执行==默认初始化==，会在空间内添加其成员变量、静态标记和方法标记；同时将成员变量置为默认初始值，name：NULL；age：0。静态标记和方法标记指向其所意值。

   <img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/20180330225459282" alt="img" style="zoom:80%;" />

   3.4.~~执行父类对象的构造函数~~，基本上不用考虑这些细节

   3.5.当前对象的==显示初始化==：例如：在类中成员变量的直接=赋值、构造代码块；

   3.6.执行==构造函数==。事实上构造函数也只是一个名字为\<init>的成员方法，在调用的时候传入this指针。"邓丽君"、16，作为形参传入，通过this找到其在堆中的地址，对此对象的name和age赋值。而"中国"，通过this找到对象在堆中的地址时，找不到country，则通过静态标记前往静态区查找，找到country并且赋值。

   3.7.此时已经完成了对象的创建，将this即0X0001赋值给栈中的p1，完成类对象的创建。

   <img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/20180330230913553" alt="img" style="zoom: 80%;" />

4. 执行```p1.show()```，通过p1去堆中找到方法标记，通过方法标记中的地址去其对应的方法区中寻找，找到了，将p1的地址作为this传入，执行。



### 目前还有一个疑惑点？每一个对象都有一份自己的方法区吗？

这个问题也困扰了我很久，查了很多资料发现，成员变量肯定是每个对象有一个，但方法是所有的对象公用一个。那么他和静态方法有什么区别呢？区别实际上就在那个隐式的this上面，静态方法没有this，可以用类名调用；成员方法用this，需要用具体对象调用并且传入那个this。

![img](https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/20180330234150489)
