---
title: 基于FSM按键消抖模块
date: 2022-11-14 19:46:07
tags:
categories: Verilog
cover: 
---

> ​	由于实际的波动开关和按键开关都是机械式的设备，开关动作来回多次抖动之后才能稳定下来，这个过程就会发生抖动，如图所示**而在FPGA内部的时钟频率很高，因此这一过程都会被FPGA当作命令处理，这是我们所不希望的**
>
> ​	这时候我们就需要使用消抖模块，目的就是出去机械开关产生的抖动信号。目前对此模块的实现有很多方法，这里采用一种比较优秀的优先状态机的实现方式。

![image-20221114195452193](https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221114195452193.png)

- 基于FSM设计的消抖电路，是利用一个10ms的非同步定时器和有限状态机来完成的，计时器每10ms（近似，下面代码可以看出）产生一个滴答使能周期信号(脉冲信号)，有限状态机则利用此信号来确定输入是否稳定。
- 在这个设计方案中，FSM将消除时间较短的抖动，当输入信号稳定后再过20ms采取`改变`输出值。

- 下面给出状态转换图：

<img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221114195914823.png" alt="image-20221114195914823" style="zoom:50%;" />

- 下面给出Verlog代码：

```verilog
module db_fsm(
    input clk,rst_n,
    input sw,
    output reg db
    );
    //定义状态
	reg[2:0] state,next_state;
    localparam[2:0]
        zero = 3'b000,
        wait1_1 = 3'b001,
        wait1_2 = 3'b010,
        wait1_3 = 3'b011,
        one = 3'b100,
        wait0_1 = 3'b101,
        wait0_2 = 3'b110,
        wait0_3 = 3'b111;
    
    /*计时器模块 start*/ 
    wire m_tick;
    localparam N = 20;
    //这里利用了寄存器超位溢出的原理，省去了一个if，
    //由于每加一都需要判断一次，因此这里省下了很多的时间复杂度
    //后果就是这样会导致滴答信号的周期为10.48ms，对于此工程影响不大
    reg[N-1:0] q_reg;
    wire[N-1:0] q_next;
    always@(posedge clk)
        q_reg<=q_next;
    assign q_next = q_reg+1;
    assign m_tick = (q_reg==0)?1'b1:1'b0;
    /*计时器模块 end*/ 
    
    /*状态机模块 start*/
    always@(posedge clk,posedge rst_n)
        if(rst_n)
            state<=zero;
        else
            state<=next_state;
    
    always@(*)begin
        next_state=state;
        db=1'b0;
        case(state)
            zero:if(sw)
                    next_state=wait1_1;
            wait1_1:begin
            if(!sw)
                next_state=zero;
            else if(m_tick)
                next_state=wait1_2;
            end
            wait1_2:begin
            if(!sw)
                next_state=zero;
            else if(m_tick)
                next_state=wait1_3;
            end
            wait1_3:begin
            if(!sw)
                next_state=zero;
            else if(m_tick)
                next_state=one;
            end
            one:begin
                db=1'b1;
                if(!sw)
                    next_state=wait0_1;
            end
            wait0_1:begin
                db=1'b1;
                if(sw)
                    next_state=one;
                else if(m_tick)
                    next_state=wait0_2;
            end
            wait0_2:begin
                db=1'b1;
                if(sw)
                    next_state=one;
                else if(m_tick)
                    next_state=wait0_3;        
            end
            wait0_3:begin
                db=1'b1;
                if(sw)
                    next_state=one;
                else if(m_tick)
                    next_state=zero;   
            end
            default:next_state=zero;

        endcase
    end
    /*状态机模块 end*/
    
        
endmodule

```

