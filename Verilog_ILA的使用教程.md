---
title: ILA的使用教程
date: 2022-11-11 10:36:29
tags:
categories: Verilog
cover:
---
### 1.ILA工具简介
​	在FPGA开发中，当完成代码设计后，为验证代码的正确性和不同条件下的可靠性，设计师往往优先想到逻辑仿真进行相关验证。
​	而使用仿真需要设计人员写出tb代码，从而增加代码的书写量，随之而产生提高验证工作的门槛和排除错误的工作量等一系列问题。而产生提高验证工作的门槛和排除错误的工作量等一系列问题。特别是对于刚刚步入FPGA殿堂的初学者，有时候很难对testbench的设计方法有准确的把握，甚至将testbench 的书写规则和设计源码的规则混淆，从而对学习源码设计产生适得其反的效果。
​	使用仿真进行逻辑验证还有另一个局限性，例如有些只有在班机上有的物理特性，很难通过仿真的方法测试。
​	最后，仿真大多适用于以时间为标尺，对条件变化有清晰的时间节点预设的验证需求。这就导致对工程的验证很难形成FPGA芯片、计算机和人的三者互动，也很难通过捕捉随机触发条件而进行验证，从而使触发条件的选择，受较大的局限性
​	能够较好解决上述问题的一种方案，是考虑使用逻辑分析仪。而采用传统的逻辑分析仪，有存在额外的设备采购开销。因此FPGA厂商推出了一定程度上能够代替逻辑分析仪的在线调试工具。

### 2.创建调试环境

### 2.1 使用IP核创建ILA调试环境

> 这种方法虽然操作流程略微复杂，但其通用性强，可以优先完全掌握此方法

<img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221111105101709.png" alt="image-20221111105101709" style="width:400px;" /><img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221111105434923.png" alt="image-20221111105434923" style="width:400px" />     <img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221111105615214.png" alt="image-20221111105615214" style="width:400px;" /><img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221111105731235.png" alt="image-20221111105731235" style="width:400px" /><img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221111110220693.png" alt="image-20221111110220693" style="width:400px" />

> 可以通过顶层模块中 模块加.的方式探寻到顶层模块下模块内的信号
>
> 例如:bin_counter.cnt 就是bin_counter模块下的，cnt内部信号



### 2.2 使用IP核在BlockDesign中创建ILA调试环境



### 2.3 使用Debug标记创建ILA调试环境





### 2.4 使用路径标记和Set up debug菜单创建ILA调试环境







### 3. 生成下载文件及联合调试验证

在添加了ILA之后进行综合仿真以及生成流文件之后，在导入bit文件的同时导入itx文件即可完成ila工具的上板，同时可以看到hardware manager界面出现了如图界面。下面我逐一介绍各个区域。

<img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221111122442778.png" alt="image-20221111122442778" style="zoom:33%;" />

- 窗口1：波形显示窗口
- 窗口2：ILA core状态控制与显示。

- 窗口3：触发条件设置（设置触发条件以让其在满足其触发条件的时候，再将板子上的数据信号采样）
