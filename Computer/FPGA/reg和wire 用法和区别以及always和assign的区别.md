# reg和wire 用法和区别以及always和assign的区别

1、从仿真角度来说，HDL语言面对的是编译器，相当于使用软件思路，此时：

&nbsp;&nbsp;&nbsp;&nbsp;wire对应于连续赋值，如assign；

&nbsp;&nbsp;&nbsp;&nbsp;reg对应于过程赋值，如always，initial；

  

2、从综合角度，HDL语言面对的是综合器，相当于从电路角度来思考，此时：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;wire型变量综合出来一般情况下是一根导线。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;reg变量在always中有两种情况：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（1）always @（a or b or c）形式的，即不带时钟边沿的，综合出来的是组合逻辑；

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（2）always @（posedge clk）形式的，即带有边沿的，综合出来一般是时序逻辑，会包含触发器（Flip-Flop）

  

3、设计中，输入信号一般来说不能判断出上一级是寄存器输出还是组合逻辑输出，对于本级来说，就当成一根导线，即wire型。而输出信号则由自己来决定是reg还是组合逻辑输出，wire和reg型都可以。但一般的，整个设计的外部输出（即最顶层模块的输出），要求是reg输出，这比较稳定、扇出能力好。

  

4、Verilog中何时要定义成wire型？

&nbsp;&nbsp;&nbsp;&nbsp;情况一：assign语句

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;例如：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;reg   a,b;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;wire  out;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;......

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;assign   out = a & b;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;如果把out定义成reg型，对不起，编译器报错！

   情况二：元件实例化时必须用wire型

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;例如：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;wire   dout;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ram   u\_ram

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;....

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.out(dout);

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;wire为无逻辑连线，wire本身不带逻辑性，所以输入什么就的输出什么。所以如果用always语句对wire变量赋值，对不起，编译器报错。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;那么你可能会问，  assign    c = a & b;   不是对wire的赋值吗？

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;事实上并非如此，综合时是将  a & b综合成 a、b经过一个与门，而c是连接到与门输出线，真正综合出来的是与门&，不是c。

  

5、何时用reg、何时用wire？

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;大体来说，reg和wire类似于C、C++的变量，但若此变量要放在begin...end之内，则该变量只能是reg型；在begin...end之外，则用wire型；

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;使用wire型时，必须搭配assign；reg型可以不用。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;input、output、inout预设值都是wire型。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在Verilog中使用reg型，并不表示综合出来就是暂存器register：在组合电路中使用reg，组合后只是net；在时序电路中使用reg，合成后才是以Flip-Flop形式表示的register触发器。

  

6、reg和wire的区别：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;reg型数据保持最后一次的赋值，而wire型数据需要持续的驱动。wire用在连续赋值语句assign中；reg用于always过程赋值语句中。

&nbsp;&nbsp;&nbsp;&nbsp;在连续赋值语句assign中，表达式右侧的计算结果可以立即更新到表达式的左侧，可以理解为逻辑之后直接连接了一条线，这个逻辑对应于表达式的右侧，这条线对应于wire；

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在过程赋值语句中，表达式右侧的计算结果在某种条件的触发下放到一个变量当中，这个变量可以声明成reg型，根据触发条件的不同，过程语句可以建模不同的硬件结构：

&nbsp;&nbsp;&nbsp;&nbsp;（1）如果这个条件是时钟上升沿或下降沿，那硬件模型就是一个触发器，只有是指定了always@（posedge or negedge）才是触发器。

&nbsp;&nbsp;&nbsp;&nbsp;（2）如果这个条件是某一信号的高低电平，那这个硬件模型就是一个锁存器。

&nbsp;&nbsp;&nbsp;&nbsp;（3）如果这个条件是赋值语句右侧任意操作数的变化，那这个硬件模型就是一个组合逻辑。

  

7、过程赋值语句always@和连续赋值语句assign的区别：

&nbsp;&nbsp;&nbsp;&nbsp;（1）wire型用于assign的赋值，always@块下的信号用reg型。这里的reg并不是真正的触发器，只有敏感列表内的为上升沿或下降沿触发时才综合为触发器。

&nbsp;&nbsp;&nbsp;&nbsp;（2）另一个区别，举例：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;wire     a;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;reg      b;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;assign     a = 1'b0;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;always@(\*)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;b = 1'b0;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;上面例子仿真时a将会是0，但是b的状态是不确定的。因为Verilog规定，always@（\*）中的\*指的是该always块内的所有输入信号的变化为敏感列表，就是说只有当always@（\*）块内输入信号发生变化，该块内描述的信号才会发生变化。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;像always@（\*）  b= 1'b0; 中由于1‘b0是个常数，一直没有变化，由于b的足组合逻辑输出，所有复位时没有明确的值--即不确定状态，又因为always@（\*）块内没有敏感信号变化，此时b信号一直保持不变，即不确定是啥，取决于b的初始状态。