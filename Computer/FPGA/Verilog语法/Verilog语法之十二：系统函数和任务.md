# Verilog语法之十二：系统函数和任务
```verilog
$finish(n);
```

系统任务$finish的作用是退出仿真器，返回主操作系统，也就是结束仿真过程。任务$finish可以带参数，根据参数的值输出不同的特征信息。如果不带参数，默认$finish的参数值为1。下面给出了对于不同的参数值，系统输出的特征信息：

*   不输出任何信息
*   输出当前仿真时刻和位置
*   输出当前仿真时刻，位置和在仿真过程中所用memory及CPU时间的统计

## **5.系统任务$stop**

格式：

```verilog
$stop;
$stop(n);
```

**$stop任务的作用是把EDA工具(例如仿真器)置成暂停模式，**在仿真环境下给出一个交互式的命令提示符，将控制权交给用户。这个任务可以带有参数表达式。根据参数值(0，1或2)的不同，输出不同的信息。参数值越大，输出的信息越多。

## **6.系统任务$readmemb和$readmemh**

在Verilog HDL程序中有两个系统任务$readmemb和$readmemh用来从文件中读取数据到存贮器中。这两个系统任务可以在仿真的任何时刻被执行使用，其使用格式共有以下六种：

**1) $readmemb("<数据文件名>",<存贮器名>);**

**2) $readmemb("<数据文件名>",<存贮器名>,<起始地址>);**

**3) $readmemb("<数据文件名>",<存贮器名>,<起始地址>,<结束地址>);**

**4) $readmemh("<数据文件名>",<存贮器名>);**

**5) $readmemh("<数据文件名>",<存贮器名>,<起始地址>);**

**6) $readmemh("<数据文件名>",<存贮器名>,<起始地址>,<结束地址>);**

在这两个系统任务中，被读取的数据文件的内容只能包含：空白位置(空格，换行，制表格(tab)和form-feeds)，注释行(//形式的和/\*...\*/形式的都允许)，二进制或十六进制的数字。

数字中不能包含位宽说明和格式说明，对于$readmemb系统任务，每个数字必须是二进制数字，对于$readmemh系统任务，每个数字必须是十六进制数字。

数字中不定值x或X，高阻值z或Z，和下划线(\_)的使用方法及代表的意义与一般Verilog HDL程序中的用法及意义是一样的。另外数字必须用空白位置或注释行来分隔开。

在下面的讨论中，地址一词指对存贮器(memory)建模的数组的寻址指针。当数据文件被读取时，每一个被读取的数字都被存放到地址连续的存贮器单元中去。存贮器单元的存放地址范围由系统任务声明语句中的起始地址和结束地址来说明，每个数据的存放地址在数据文件中进行说明。当地址出现在数据文件中，其格式为字符“@”后跟上十六进制数。如：

```text
 @hh...h
```

对于这个十六进制的地址数中，允许大写和小写的数字。在字符“@”和数字之间不允许存在空白位置。可以在数据文件里出现多个地址。当系统任务遇到一个地址说明时，系统任务将该地址后的数据存放到存贮器中相应的地址单元中去。

对于上面六种系统任务格式,需补充说明以下五点：

1) 如果系统任务声明语句中和数据文件里都没有进行地址说明，则缺省的存放起始地址为该存贮器定义语句中的起始地址。数据文件里的数据被连续存放到该存贮器中，直到该存贮器单元存满为止或数据文件里的数据存完。

2) 如果系统任务中说明了存放的起始地址，没有说明存放的结束地址，则数据从起始地址开始存放，存放到该存贮器定义语句中的结束地址为止。

3) 如果在系统任务声明语句中，起始地址和结束地址都进行了说明，则数据文件里的数据按该起始地址开始存放到存贮器单元中，直到该结束地址，而不考虑该存贮器的定义语句中的起始地址和结束地址。

4) 如果地址信息在系统任务和数据文件里都进行了说明，那么数据文件里的地址必须在系统任务中地址参数声明的范围之内。否则将提示错误信息，并且装载数据到存贮器中的操作被中断。

5) 如果数据文件里的数据个数和系统任务中起始地址及结束地址暗示的数据个数不同的话，也要提示错误信息。

下面举例说明：

先定义一个有256个地址的字节存贮器 mem:

```verilog
 reg[7:0] mem[1:256];
```

下面给出的系统任务以各自不同的方式装载数据到存贮器mem中。

```verilog
initial $readmemh("mem.data",mem);
initial $readmemh("mem.data",mem,16);
initial $readmemh("mem.data",mem,128,1);
```

第一条语句在仿真时刻为0时，将装载数据到以地址是1的存贮器单元为起始存放单元的存贮器中去。

第二条语句将装载数据到以单元地址是16的存贮器单元为起始存放单元的存贮器中去，一直到地址是256的单元为止。

第三条语句将从地址是128的单元开始装载数据，一直到地址为1的单元。在第三种情况中，当装载完毕，系统要检查在数据文件里是否有128个数据，如果没有，系统将提示错误信息。

## **7.系统任务 $random**

这个系统函数提供了一个产生随机数的手段。当函数被调用时返回一个32bit的随机数。它是一个带符号的整形数。

$random一般的用法是：$ramdom % b ,其中 b>0.它给出了一个范围在（-b+1):(b-1)中的随机数。下面给出一个产生随机数的例子：

```verilog
reg[23:0] rand;
rand = $random % 60;
```

上面的例子给出了一个范围在**－59到59之间**的随机数，

下面的例子通过位并接操作产生一个值在**0到59之间**的数。

```verilog
reg[23:0] rand;
rand = {$random} % 60;
```

利用这个系统函数可以产生随机脉冲序列或宽度随机的脉冲序列，以用于电路的测试。

下面例子中的Verilog HDL模块可以产生宽度随机的随机脉冲序列的测试信号源,在电路模块的设计仿真时非常有用。同学们可以根据测试的需要，模仿下例，灵活使用$random系统函数编制出与实际情况类似的随机脉冲序列。

\[例\]

```verilog
`timescale 1ns/1ns
module random_pulse( dout );
output [9:0] dout;
reg dout;
integer delay1,delay2,k;

initial 
    begin
        #10 dout=0;
        for (k=0; k< 100; k=k+1)
            begin 
                delay1 = 20 * ( {$random} % 6); 
                // delay1 在0到100ns间变化
                delay2 = 20 * ( 1 + {$random} % 3); 
                // delay2 在20到60ns间变化
                #delay1 dout = 1 << ({$random} %10);
                //dout的0--9位中随机出现1，并出现的时间在0-100ns间变化
                #delay2 dout = 0;
                //脉冲的宽度在在20到60ns间变化
            end
    end
endmodule
```