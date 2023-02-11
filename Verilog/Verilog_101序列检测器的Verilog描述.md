---
title: 101序列检测器的Verilog描述（Moore&Mealy）
date: 2022-11-07 08:52:41
tags:
categories: Verilog
cover:
---

> 序列检测器
> 1) 设计“1101”序列检测的状态转换图;
> 2) 设计一个 8 位并转串输出模块 par2ser。该器件有 8 位输入 d[7:0]，1 位输出 q，另有一个 clk 端，一个 set 端。set端上升沿将 8位输入锁存到逻辑右移移位寄存器中。
> 3) 调用并转串输出模块，使用Verilog HDL语言的行为描述方式实现一个摩尔状态机，能检测一个8位的二进制数据中是否存在“1101”序列，如果检测到该序列则指定的LED灯亮；
> 4) 综合、实现、生成bit流，下载到Nexys4开发板进行验证； 

> 经典的有限状态机三段式设计

### Moore型

```verilog
always@(poesedge clk or rst_n) begin
    if(clr) state<=S0;
    else state<=next_state;//当时钟来的时候，将next_state(通过一系列的组合逻辑从激励和输入中得到的次态)赋给state
end




always@(state or x) begin //当前的输入和现态决定了次态,在别的情况下也可以是输入和激励得到次态，，
    case(state)
        S0:begin if(x) next_state=S1;
            else next_state = S0;end//注意，这里是组合逻辑，用的是阻塞赋值
        S1:begin if(x) next_state=S2;
            else next_state = S2;end
        S2:begin if(x) next_state=S3;
            else next_state = S0;end
        S3:begin if(x) next_state=S1;
            else next_state = S2;end
        default:next_state = S0;//当目前的现态和输入是不合法的，则回到S0从新开始
    endcase
end


always@(state)//输出逻辑，moore型的输出仅仅与其现态有关
    begin case(state)
        S3:z=1'b1;
    	default:z=1'b0;
    endcase
end

```

仿真代码:

```verilog
`timescale 1ns / 1ps

module more1101detector_tb();
reg clk;
reg rst_n;
reg in;
wire flag;
more1101Detector inst_01(
    .clk(clk),
    .rst_n(rst_n),
    .in(in),
    .flag(flag)
    );
    
    initial begin
        clk=1;
        rst_n=0;#100
        rst_n=1;#200
        rst_n=0;
        
    end
    always #500 clk=!clk;//时钟周期为1000ns
    
    reg[7:0] data;
    
    //利用always块使每个时钟周期，左移一位并且发送一个数据给1101检测机
    
    
    /*这里存在一个问题，到底是先左移还是先赋值给in???
    若我们用c语言的逻辑来看，左移在上，赋值在下，左移先于赋值，但这是错误的，注意到这是非阻塞赋值，右边同时操作：左移(此时并未真正左移，没有在D触发器中通过，因此data还没有变化)，data[7]已经准备好。直到大家都准备好以后，一起通过触发器走到赋值语句左侧
    */
    always@(posedge clk or posedge rst_n)begin
        if(rst_n)
            data <= 8'b10110110;
        else
            data <= {data[6:0],data[7]};//左移一位
    end

    always@(posedge clk or posedge rst_n)begin
        if(rst_n)
            in <= 1'b0;
        else
            in <= data[7];//必须在时钟的上升沿给其数据才有效
    end

endmodule
```

### Mealy型

```verilog
module more1101Detector(
    clk,
    rst_n,
    in,
    flag
    );
    input clk;
    input rst_n;
    input in;
    output reg flag;
    parameter S0 = 3'b000;
    parameter S1 = 3'b001;
    parameter S2 = 3'b010;
    parameter S3 = 3'b011;
    parameter S4 = 3'b100;
    reg [2:0] state;
    reg [2:0] next_state;
    
    always@(posedge clk or posedge rst_n) begin
        if(rst_n) state<=S0;
        else state<=next_state;
    end
    
    always@(state or in) begin //现态和输入决定了次态
        case(state)
            S0:begin  if(in) next_state=S1;
            else next_state=S0;end
            S1:begin  if(in) next_state=S2;
            else next_state=S0;end
            S2:begin  if(in) next_state=S2;
            else next_state=S3;end
            S3:begin  if(in) next_state=S4;
            else next_state=S0;end
            S4:begin  if(in) next_state=S2;
            else next_state=S0;end     
            default:next_state=S0;       
        endcase
    end
    

    always@(state,in)//Mealy型与现态和输入有关
        if(state==S3&&in==1'b1)
            flag=1'b1;
        else
            flag=1'b0;
    

```

### 那么Moore和Mealy型的区别在哪里？

1. 本质概念上的区别:Moore型当前的输出仅与现态有关，而Mealy型的输出于当前的输入和现态有关。
2. 同一个状态Mealy型比Moore型超时一个周期输出。Moore型的输出只由当前状态决定，因此当前输入只能在下一周期才能发挥作用。
