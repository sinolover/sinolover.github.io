# verilog语法学习0：常用基础语法全梳理（上）
今天把前面看过的夏宇闻老师的那本书上面的verilog的常用语法进行了一下梳理，并且为了熟悉语法在HDLBits刷了刷题，一些例子就直接从那个里面引用了。参考过的内容列在下面：

*   verilog数字系统设计教程（夏宇闻）：学习verilog的必读书。第四版的第三章到第七章，夏宇闻老师这本书上讲的比较详细。
*   [HDLBits](https://link.zhihu.com/?target=https%3A//hdlbits.01xz.net/wiki/Main_Page)：类似于CS的LeetCode，题目倒是没有LeetCode多，适合入门语法。
*   [Verilog Tutorial](https://link.zhihu.com/?target=https%3A//www.chipverify.com/verilog/verilog-tutorial) : 这个里面对verilog语法讲的很细，而且有大量的代码示例，不过代码不能直接复制下来运行，但是用一些小trick还是可以把代码搞下来的。

## 一、verilog模块的结构

verilog语法中最基本的元素就是模块了，主要包括模块声明以及模块内容。首先是模块的声明，具体的声明结构如下

```verilog
module Hello_World(<端口信号列表>); //注意这里要有分号
	<逻辑代码>
endmodule
```

接下来是模块内容，主要包括I/O口的说明，内部信号的说明，功能的定义三个结构。

**（1）I/O口的说明**

```verilog
输入端口：
	input[信号位宽-1:0] 端口1;
        input[信号位宽-1:0] 端口2;
        ...
输出端口：
	output[信号位宽-1:0] 端口1;
        output[信号位宽-1:0] 端口2;
        ...
输入/输出端口：
	inout[信号位宽-1:0] 端口1;
        inout[信号位宽-1:0] 端口2;
        ...
```

**（2）内部信号的说明**

```verilog
reg [width-1:0] 变量1,变量2,...;
wire [width-1:0] 变量1,变量2,...;
```

**（3）功能定义**

主要有三种最基本的功能定义方法，分别是always，assign，initial。一个module里面可以写多个always，assign，initial，这些功能在电路通电之后也是同时开始执行的。

*   initial用在**仿真中。**主要有以下两种功能，一方面可以在仿真开始时对各变量进行初始化，这个初始化过程不需要任何仿真时间，即在 0 ns 时间内，便可以完成初始化工作；另一方面可以用来生成激励波形作为电路的测试仿真信号。

```verilog
// 完成初始化
initial begin
	areg = 0;    //初始化一个寄存器
	for(index=0;index<size;index=index+1)
		memory[index] = 0; //初始化一个memory
end
// 完成激励波形的生成
initial begin
	inputs = 8'b0000_0000;
	#10 inputs = 8'b0110_0101;
	#10 inputs = 8'b1111_0101;
end
```

*   assign用于实现组合逻辑，直接互联不同的信号，或者变量值。[HDLBits：Wire](https://link.zhihu.com/?target=https%3A//hdlbits.01xz.net/wiki/Wire) ，[HDLBits：Conditional](https://link.zhihu.com/?target=https%3A//hdlbits.01xz.net/wiki/Conditional) 。

```verilog
assign <wire 变量名> = <变量或者常量>;
// 右边也可以使用 (判断条件)?(判断条件为真时的逻辑处理):(判断条件为假时的逻辑处理)
// 完成更加简洁的if...else语句
```

*   always语句可以实现组合逻辑，也可以实现时序逻辑。它是一直运行的，但是后面跟着的过程块是否执行，就要看它的触发条件是否满足了，这个触发条件就写在括号中。可以由信号的边沿触发，也可以由信号的变化触发。[HDLBits：Alwaysblock1](https://link.zhihu.com/?target=https%3A//hdlbits.01xz.net/wiki/Alwaysblock1) ， [HDLBits：Alwaysblock2](https://link.zhihu.com/?target=https%3A//hdlbits.01xz.net/wiki/Alwaysblock2) 。

```verilog
// 所有信号变换都可以触发，是组合逻辑
always@(*) begin
	...
end
// 信号a与信号b的变化都可以触发，是组合逻辑
always@(a or b) begin
	...
end
// 时钟的上升沿或者复位信号的下降沿可以触发，是时序逻辑
always@(posedge clk or negedge rst_n) begin
	...
end
```

**（4）引用模块**

在引用时既可以按照模块定义的端口顺序来连接，不用标明原模块定义时规定的端口名，也可以在引用时采用“.”符号，这样就不用完全按照模块定义的端口顺序来连接了，一般还是使用“.”来引用比较好，这样逻辑比较清晰。

```verilog
...
MyDesign M1(.sin(in),.pout(out));
...
```

MyDesign 是已经定义的另一个模块，M1是自己取的一个名字，sin与pout是MyDesign模块里面端口的名字，in与out是在正在写的模块中定义的两个信号，这两个信号分别连接到sin与pout端口。[HDLBits：Module](https://link.zhihu.com/?target=https%3A//hdlbits.01xz.net/wiki/Module) 。

## 二、数据类型

数据主要可以分为两种类型，一种是常量类型的数据，另一种是变量类型的数据。

**（1）常量类型的数据**

*   整数：二进制（b），十进制（d），十六进制（h）。
*   x和z值：

*   x代表未定值：不确定状态为高还是为低，旨在告诉综合工具设计者不关心它的电平是多少，是0是1都可以。
*   z代表高阻值：输出引脚与内部电路断开，不导通，其电平随外部电平高低而定，表示设计者不驱动这个信号。
*   **一个x可以用来定义十六进制数的4位二进制数的状态或者二进制数的1位。建议在写case语句中使用这些值。**

*   负数：在位宽表达式前加一个减号就可以定义一个负数。
*   下划线：主要用来分隔开数的表达式以提高程序的可读性，它只能用在具体的数字之间。

```verilog
<位宽><进制><数字>
8'b10101100 // 位宽为8的数的二进制表示，'b表示二进制
8'ha2 // 位宽为8的数的十六进制表示，'h表示十六进制
<数字>
10 //默认采用十进制，并且数字的位宽采用默认位宽（与机器系统相关，最少32位）

4'b10x0  //位宽为4的二进制数，从低位数起第2位是不定值
8'h4x    //位宽为8的二进制数，其低四位值为不定值

-8'd5 //这个表达式代表5的补数

16'b1010_1011_1111_1010
```

*   参数：使用parameter来定义常量，注意在引用实例的时候可以通过参数的传递来改变定义时规定的参数。

```verilog
//parameter可以直接定义一系列常量参数
parameter a=2,b=2.3,c=a-1;

//可以通过传参修改module中定义的parameter
module Decode(A,B);
	parameter width=1,polarity=1;
	...
endmodule
module Decode_top;
	wire[3:0] A1;
	wire[3:0] B1;
	wire[3:0] A2;
	wire[3:0] B2;
	Decode #(4,0) D1(A1,B1); // D1实例引用的width=4，polarity=0
	Decode #(5) D2(A2,B2);   // D2实例引用的width=5，polarity=1
endmodule
```

**（2）变量类型的数据**

*   wire型：常用来表示用以assign关键字指定的组合逻辑信号。verilog程序模块中输入、输出信号类型默认时自动定义为wire型。wire型信号可以用做任何方程式的输入，也可以用作assign语句的输出。
*   reg型：常用来表示always模块内的指定信号，在always模块内被赋值的每一个信号都必须定义成reg型。reg的默认初始值是不定值。
*   memory型：从编程角度可以理解成一个多维数组，从物理角度可以理解成RAM型存储器或者ROM存储器，从实现角度可以理解成是reg型数据的扩展。 [HDLBits：Vector1](https://link.zhihu.com/?target=https%3A//hdlbits.01xz.net/wiki/Vector1) 。

```verilog
wire [3:0] a,b; //定义了两个4位的wire型数据
reg [3:0] a,b;  //定义了两个4位的reg型数据
reg [n-1:0] memory [m-1:0] //[n-1:0]定义存储器中每一个存储单元的大小，[m-1:0]定义了
			   //存储器中存储单元的个数也可以理解成各存储单元的地址
memory[3] = 0; //注意对memory中的存储单元进行读写操作的时候必须指定该单元在存储器中的地址
```

## 三、运算符及表达式

*   算术运算符

```verilog
+  //加
-  //减
*  //乘
/  //除 
%  //求模
```

进行整数除法运算时结果值略去小数部分，只留下整数部分，取模运算要求%两侧均为整数。

*   位运算符

```verilog
~  //取反
&  //按位与
|  //按位或
^  //按位异或
~^ //按位同或
```

不同长度的数据在进行位运算的时候，系统会自动地将两者按右端对齐，位数少的操作数会在相应的高位用0填满。

*   逻辑运算符

```verilog
&& //逻辑与
|| //逻辑或
!  //逻辑非
```

*   关系运算符

```verilog
a<b   //小于
a>b   //大于
a<=b  //小于等于
a>=b  //大于等于
```

*   等式运算符

```verilog
==  //等于
!=  //不等于
```

*   移位运算符

```verilog
a>>n  //a右移n位
a<<n  //a左移n位
```

两种移位运算都用0来填补移出的空位。

*   位拼接运算符

```verilog
{信号1的某几位,信号2的某几位,...}
{a,b[3:0],w,3'b101}
{4{w}} //位拼接可以用来简化表达式，这样写等同于{w,w,w,w}
{b,{3{a,b}}} //这样写等同于 {b,a,b,a,b,a,b}
```

位拼接表达式不允许存在没有指明位数的信号。[HDLBits：Vector3](https://link.zhihu.com/?target=https%3A//hdlbits.01xz.net/wiki/Vector3) 。

*   缩减运算符

```verilog
reg [3:0] B;
reg C;
C = &B; // 相当于 C=((B[0] & B[1])&B[2])&B[3]
```

与之前的位运算不同，缩减运算是对单个操作数各位的逻辑操作，最终的运算结果是1位二进制数。

## 四、条件语句及循环语句

**（1）条件语句**

条件语句主要有两种写法，一种是if else的写法，另一种是case的写法。

```verilog
// if else 的写法
if(a==2'b00) begin
	<具体逻辑>
end
else if(a==2'b01) begin
	<具体逻辑>
end
else begin
	<具体逻辑>
end
// case 的写法
case(a)
	2'b00:<具体逻辑>
	2'b01:<具体逻辑>
	default:<具体逻辑>
endcase
```

还有用法与case类似的casex与casez，这两者可以用来处理比较过程中不必考虑的情况。其中casez语句用来处理不必考虑高阻值z的比较过程，casex语句则将高阻值z和不定值都视为不必关心的情况。所谓不必关心的情况就是说，在表达式进行比较时不将该位的状态考虑在内。 [HDLBits：Always case](https://link.zhihu.com/?target=https%3A//hdlbits.01xz.net/wiki/Always_case) ，[HDLBits：Always case2](https://link.zhihu.com/?target=https%3A//hdlbits.01xz.net/wiki/Always_case2) ，[HDLBits：Always casez](https://link.zhihu.com/?target=https%3A//hdlbits.01xz.net/wiki/Always_casez) ，[HDLBits：Always if2](https://link.zhihu.com/?target=https%3A//hdlbits.01xz.net/wiki/Always_if2) 。

**（2）循环语句**

总体来说循环语句用到的不是很多，这里就列举两个最常用的forever和for循环。

*   forever循环语句常用来产生周期性的波形，用来作为仿真信号，必须要写在initial块中。
*   for循环语句的一般形式是for（<变量名>=<初值>；<判断表达式>；<变量名>=<新值>）。for可以综合的，for几次就相当于把你的电路复制几次。[HDLBits：Vector100r](https://link.zhihu.com/?target=https%3A//hdlbits.01xz.net/wiki/Vector100r) 。

```verilog
initial begin
   clk = 0;
   forever begin
       clk = ~clk;
       #5;
   end
end

initial begin
    counter2 = 'b0 ;
    for (i=0; i<=10; i=i+1) begin
        #10 ;
        counter2 = counter2 + 1'b1 ;
    end
end
```

  