---
title: Java学习阶段笔记
date: 2020-05-19 18:08:25
tags:
categories: Java
cover:
---

## 一、JDK、JRE和JVM

### 1.JDK：

JDK,全称Java Development Kit，意为**Java程序开发的工具包**。内部包括JVM,核心类库(包括String，System等基础库)，以及开发工具（例如javac编译工具，java运行工具，jdb调试工具，jhat内存分析工具...）

<img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221119182011103.png" alt="image-20221119182011103" style="zoom:50%;" />

### 2.JRE:

JRE,全称Java Runtime Environment，意为**Java程序的运行环境**。内部包括JVM，核心类库，运行工具。*(与JDK相比占用内存较小)*

<img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221119182834542.png" alt="image-20221119182834542" style="zoom:50%;" />

### 3.JVM：

JVM，全称Java Virtual Machine，意为Java运行虚拟机。

它是java实现跨平台的最核心的一部分，他负责执行字节码文件，是可运行Java字节码文件的虚拟计算机。所有平台（包括X86，ARM....）上的JVM向编译器提供相同的接口，而编译器只需要面向虚拟机即可。



## 二、内存图

### Java的内存分配

- 栈       方法运行时使用的内存（包括一些基本数据类型变量的定义...）
- 堆       存储对象或者是数组，用new来创建的，都存储在堆内存（用new来创建的都有自己的一套定义机制）
- 方法区    存储可以运行的class文件
- 本地方法栈 JVM在使用操作系统功能的时候使用
- 寄存器     CPU使用的内存区间



### 数组的内存图

![image-20221119183408778](https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221119183408778.png)

1. 在main方法被调用即将其压入栈内存；
2. 执行int[] arr = new int[2]时:
   1. ```int[] arr```会在对应方法下的栈内存中定义一个int[]类型的变量arr；
   2. ```new int[2]```会在堆内存中开辟一片空间，下面有一个length属性以及两个int的空间；
   3. 而```=```将new返回的空间的地址赋值给了int[]类型的变量arr；
   4. 因此事实上数组变量保存了数组在堆空间的地址；
3. 最后，执行结束，main方法出栈；

### 方法的内存图

![image-20221119191141520](https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221119191141520.png)

方法在每次调用的时候都会入栈，而当到达方法调用结束标识(‘}’)时，对应的方法就会从栈中弹出。（每次只会弹出栈顶的那个方法）然后回到自己的调用处，继续往下执行，因此会在调用方法的时候携带一个记录位置的指针。

### :star2:==对象内存图:star2:==



![image-20221120142437120](https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221120142437120.png)

详见另一篇博客：[Java中对象的内存图解 | SaladDay的Blog](https://www.saladday.top/2022/11/25/Java_Java中对象的内存图解/)





### String的内存图

1. 直接赋值的方式

<img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221120182324091.png" alt="image-20221120182324091" style="zoom:50%;" />

> 在执行到```String s1 = "abc"```的时候，虚拟机会观察在StringTable中有无此字符串，有则直接返回地址，无则创建
>
> 节省内存；

2. new关键字获取

<img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221120182439631.png" alt="image-20221120182439631" style="zoom:50%;" />

> 在执行到```String s1 = new String(chs)```时，虚拟机现在栈中创建一个s1的变量，再在堆内存中创建空间并且初始化，将地址赋值给s1；
>
> 这样创建出来的字符串无复用性，占用内存；

### 继承的内存图

<img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221122001027920.png" alt="image-20221122001027040" style="zoom:50%;" />





:star: 下面逐行代码来分析：

- ```public class TestStudent```将TestStudent.class放入方法区（内部含有main方法）,自动运行main方法，将其推入栈内存
- ```Zi z = new Zi() ```
  - 首先用到了Zi，将其放入方法区当中，检测到其继承了Fu，将Fu.class放入方法区。(还有默认继承的Object)
  - 执行Zi z,在main方法中创建一个空间，命名为z，并且限定其内容为Zi。
  - 执行new Zi()，在堆空间中创建一个空间，分为两块，分别将Zi和Fu类的属性和方法放入其中。同时将地址赋值给z
- ```z.name = ”钢门吹雪“```通过z中的地址到堆空间中索引，会首先到子类空间去找，未找到则父空间去找，由于name为privata，则其找不到，报错；
- 最后执行完毕，main出栈，Java的垃圾回收机制会把堆空间中的垃圾空间释放；







## 三、集合：

### 1.集合和数组的对比

- 长度：
  - 数组的长度是固定的
  - 集合的长度是可变的
- 存储类型
  - 数组可以存基本数据类型和引用数据类型(地址)
  - 集合只可以存引用数据类型，若要存基本数据类型，需要将其变为包装类



## 四、Java小知识点

### 1.类的构造方法

> 构造方法就是创建对象吗？
>
> 答案是否定的，事实上真正的创建对象在于new关键字，构造方法只不过是创建对象的一个小步骤而已，这小步骤的目的是**给成员变量初始化**。

### 2.成员变量和局部变量的区别

成员变量：放在堆内存中，于类中的，是**有初始值**的；

局部变量：放在栈内存中，于方法中的，是**没有初始值**的；

### 3.static静态关键词

> static修饰的方法和属性，是放在静态区内的，在对象被创建前（在类定义时）就被创建好。是属于类的，不属于任何一个具体对象。

<img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221121192619784.png" alt="image-20221121192619784" style="zoom:33%;" />

当我们在类中创建非static方法的时候，第六行的代码实际上是

```public void print(User this)```

会把调用此方法的对象的地址传入

---



:heart:static静态的注意事项：

1. 静态方法中没有this关键字
2. 静态方法不能访问成员变量和其他非静态方法
3. 非静态方法可以访问所有

静态方法在被调用的时候并没有传入this(他不属于任何一个对象)，因此他不能访问成员变量；并且若要调用非静态方法，也要传入一个this进入，而静态方法自己都没有this，哪来的this给别人，因此也是不可以调用的；:black_flag:事实上，静态方法只可以访问位于静态区中的内容！

---

:heart:**重新认识main方法**

```java
public class text{
    public static void main(string[] args){
     ----
    }
}
```

- public:    被JVM调用，访问权限需要够大

- static:    被JVM调用，不需要创建text对象就可以直接调用（:star:因为main方法是静态的，静态的方法不可以调用非静态方法，因此若在此类中定义其他方法也应该是静态的)
- void:      被JVM调用，不需要给JVM返回值
- main:      一个通用的名词，虽然不是关键字，但是可以被JVM识别
- String[]   以前用于接受键盘录入数据的，已弃用。为保证程序的向下兼容，在高版本的代码中依旧保留。

### 4.==号的比较规则

- 基本数据类型：比较的是数据值（真值）
- 引用数据类型：比较的是地址值

### 5.String拼接

```java
String s1= "a";
String s2 = s1+"bc";
```

分为两种情况：

1. JDK8以前

此时的第二行代码的完整形式应该是

```java
s2 = new StringBuilder().append(s1).append("bc").toSting();
```

2. JDK8以后

系统会预估需要凭借字符串的长度，生成一个数组，利用数组产生字符串(new出来的)

### 6.类的继承

继承的实现：

1. 在方法区当中，父类将==所有==的成员变量、虚方法表(非static、非final、非private)传递给子类；
2. 在创建子类对象的时候，将所有的子类与父类的成员变量创建，**不过无法找到父类中私有的成员变量**

### 7.多态

 1. 多态的优势

	方法中，使用父类型作为参数，可以接受所有子类对象

 2. 多态的弊端是什么

	不能使用子类型的特有功能，若需要使用，则必须要用强制类型转换。

### 8.权限修饰符

| 修饰符    | 同一个类中 | 同一个包中其他类 | 其他包中的子类 | 其他包中的无关类 | 记忆                                   |
| --------- | ---------- | ---------------- | -------------- | ---------------- | -------------------------------------- |
| private   | √          |                  |                |                  | 私房钱                                 |
| default   | √          | √                |                |                  | 钱给老婆，家里的能用，外边的孩子不能用 |
| protected | √          | √                | √              |                  | 外边的孩子也是受法律保护的，也能用     |
| public    | √          | √                | √              | √                | 大家都可以用                           |

### 9.抽象类

- 抽象方法：将==共性的==方法抽取到父类之后，由于每个子类执行的内容是不一样的，所以，在父类中不能确定具体的方法体。这种==不能确定方法体==的方法就称为抽象方法；
- 抽象类：如果一个类中有抽象方法，那么这个类就必须定义为抽象类：
  - 抽象类不能实例化；
  - 可以有构造方法；

### 10.接口

- 为什么有接口？

<img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221124113223022.png" alt="image-20221124113223022" style="zoom: 50%;" />

接口是一种对方法共性的描述，是一种规则，对继承其的子类的方法的规制（必须写，且格式必须相同）

==注意:==其可以多继承，一个类可以继承多个接口，但一个类不能继承多个父类；

### :star:区分：抽象类和构造方法私有的类的区别：

1.抽象类不可以被实例化，但其是要被继承的，是要被使用到的类实现的；

2.构造方法私有虽然不可以被实例化，但是其可以有内部的实例构造。其次他是不可以被继承的。是为了实现所谓的单构造模式（全局只有一个实例）。
