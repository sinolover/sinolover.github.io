# Verilog语法之〇：Verilog HDL简介

众所周知，学习FPGA必须先掌握一门硬件描述语言，所以我为初学者们将Verilog语法进行了总结，写了十三篇文章。

Verilog HDL的语法与C语言的语法有许多类似的地方，但也有许多不同的地方。我们学习Verilog HDL语法要善于找到不同点，着重理解如：

*   阻塞〔Blocking〕和非阻塞〔Non-Blocking〕赋值的不同；
*   顺序块和并行块的不同；
*   块与块之间的并行执行的概念；
*   task和function的概念等等。

Verilog HDL还有许多系统函数和任务也是C语言中没有的如:$monitor、$readmemb、$stop等等，而这些系统任务在调试模块的设计中是非常有用的，我们只有通过阅读大量的Verilog调试模块实例，经过长期的实践，经常查阅Verilog语言参考手册才能逐步掌握。

**Verilog HDL是一种用于数字逻辑电路设计的语言**。用Verilog HDL描述的电路设计就是该电路的Verilog HDL模型。

Verilog HDL既是一种**行为描述**的语言也是一种**结构描述**的语言。这也就是说，既可以用电路的功能描述也可以用元器件和它们之间的连接来建立所设计电路的Verilog HDL模型。

Verilog模型可以是实际电路的不同级别的抽象。这些抽象的级别和它们对应的模型类型共有以下五种：

*   **系统级**(system):用高级语言结构实现设计模块的外部性能的模型。
*   **算法级**(algorithm):用高级语言结构实现设计算法的模型。
*   **RTL级**(Register Transfer Level):描述数据在寄存器之间流动和如何处理这些数据的模型。
*   **门级**(gate-level):描述逻辑门以及逻辑门之间的连接的模型。
*   **开关级**(switch-level):描述器件中三极管和储存节点以及它们之间连接的模型。

一个复杂电路系统的完整Verilog HDL模型是由若干个Verilog HDL模块构成的，每一个模块又可以由若干个子模块构成。其中有些模块需要综合成具体电路，而有些模块只是与用户所设计的模块交互的现存电路或激励信号源。

利用Verilog HDL语言结构所提供的这种功能就可以构造一个模块间的清晰层次结构来描述极其复杂的大型设计，并对所作设计的逻辑电路进行严格的验证。

Verilog HDL行为描述语言作为一种结构化和过程性的语言，其语法结构非常适合于算法级和RTL级的模型设计。这种行为描述语言具有以下功能：

*   · 可描述顺序执行或并行执行的程序结构。
*   · 用延迟表达式或事件表达式来明确地控制过程的启动时间。
*   · 通过命名的事件来触发其它过程里的激活行为或停止行为。
*   · 提供了条件、if-else、case、循环程序结构。
*   · 提供了可带参数且非零延续时间的任务(task)程序结构。
*   · 提供了可定义新的操作符的函数结构(function)。
*   · 提供了用于建立表达式的算术运算符、逻辑运算符、位运算符。

  

·Verilog HDL语言作为一种结构化的语言也非常适合于门级和开关级的模型设计。因其结构化的特点又使它具有以下功能：

1.  提供了完整的一套组合型原语(primitive);
2.  提供了双向通路和电阻器件的原语;
3.  可建立MOS器件的电荷分享和电荷衰减动态模型。

Verilog HDL的构造性语句可以精确地建立信号的模型。这是因为在Verilog HDL中，提供了延迟和输出强度的原语来建立精确程度很高的信号模型。

信号值可以有不同的的强度，可以通过设定宽范围的模糊值来降低不确定条件的影响。

Verilog HDL作为一种高级的硬件描述编程语言，有着类似C语言的风格。其中有许多语句如：if语句、case语句等和C语言中的对应语句十分相似。如果读者已经掌握C语言编程的基础，那么学习Verilog HDL并不困难，我们只要对Verilog HDL某些语句的特殊方面着重理解，并加强上机练习就能很好地掌握它，利用它的强大功能来设计复杂的数字逻辑电路。

后面的十三章文章我们将对Verilog HDL中的基本语法逐一加以介绍。