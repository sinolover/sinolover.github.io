# Verilog语法之九：循环语句
在Verilog HDL中存在着**四种类型的循环语句**，用来控制执行语句的执行次数。

1) **forever** 连续的执行语句。

2) **repeat** 连续执行一条语句 n 次。

3) **while** 执行一条语句直到某个条件不满足。如果一开始条件即不满足(为假)，则语句一次也不能被执行。

4) **for**通过以下三个步骤来决定语句的循环执行。

*   a) 先给控制循环次数的变量赋初值。
*   b) 判定控制循环的表达式的值，如为假则跳出循环语句，如为真则执行指定的语句后，转到第三步。
*   c) 执行一条赋值语句来修正控制循环变量次数的变量的值，然后返回第二步。

下面对各种循环语句详细的进行介绍。

## **1.forever语句**

forever语句的格式如下：

```verilog
 forever 语句; 

或

forever 
   begin 
   多条语句 
   end
```

**forever循环语句常用于产生周期性的波形，用来作为仿真测试信号。它与always语句不同处在于不能独立写在程序中，而必须写在initial块中。**

## 2.repeat语句

repeat语句的格式如下:

```verilog
repeat(表达式) 语句；
 
或

repeat(表达式) 
    begin 
    多条语句 
    end
```

**在repeat语句中，其表达式通常为常量表达式。**

下面的例子中使用repeat循环语句及加法和移位操作来实现一个乘法器。

```verilog
parameter size=8,longsize=16;
reg [size:1] opa, opb;
reg [longsize:1] result;
 
begin: mult
reg [longsize:1] shift_opa, shift_opb；
shift_opa = opa;
shift_opb = opb;
result = 0;

repeat(size)
    begin
        if(shift_opb[1])
            result = result + shift_opa; 
            shift_opa = shift_opa <<1;
            shift_opb = shift_opb >>1;
    end
end
```

## **3.while语句**

while语句的格式如下：

```verilog
 while(表达式) 语句
```

或用如下格式：

```verilog
while(表达式) 
    begin 
    多条语句 
    end
```

下面举一个while语句的例子，该例子用while循环语句对rega这个八位二进制数中值为1的位进行计数。

```verilog
begin: count1s
reg[7:0] tempreg;
count=0;
tempreg = rega;

while(tempreg)
   begin
       if(tempreg[0]) count = count + 1;
       tempreg = tempreg>>1;
   end
end
```

## **4.for语句**

for语句的一般形式为：

```verilog
 for（表达式1；表达式2；表达式3)  语句
```

它的执行过程如下：

1) 先求解表达式1；

2) 求解表达式2，若其值为真（非0)，则执行for语句中指定的内嵌语句，然后执行下面的第3步。若为假(0)，则结束循环，转到第5步。

3) 若表达式为真，在执行指定的语句后，求解表达式3。

4) 转回上面的第2步骤继续执行。

5) 执行for语句下面的语句。

for语句最简单的应用形式是很易理解的，其形式如下：

```verilog
for(循环变量赋初值；循环结束条件；循环变量增值)
   执行语句
```

for循环语句实际上相当于采用while循环语句建立以下的循环结构：

```verilog
begin
    循环变量赋初值；
    while(循环结束条件)
        begin
            执行语句
            循环变量增值;
        end
end
```

这样对于需要8条语句才能完成的一个循环控制，for循环语句只需两条即可。

下面分别举两个使用for循环语句的例子。例1用for语句来初始化memory。例2则用for循环语句来实现前面用repeat语句实现的乘法器。

\[例1\]：

```verilog
begin: init_mem
reg[7:0] tempi;

for(tempi=0;tempi<memsize;tempi=tempi+1)
memory[tempi]=0;
end
```

\[例2\]：

```verilog
parameter size = 8, longsize = 16;
reg[size:1] opa, opb;
reg[longsize:1] result;
 
begin:mult
integer bindex;
result=0;
for( bindex=1; bindex<=size; bindex=bindex+1 )
    if(opb[bindex])
    result = result + (opa<<(bindex-1));
end
```

在for语句中，循环变量增值表达式可以不必是一般的常规加法或减法表达式。下面是对rega这个八位二进制数中值为1的位进行计数的另一种方法。见下例：

```verilog
begin: count1s
    reg[7:0] tempreg;
    count=0;
    for( tempreg=rega; tempreg; tempreg=tempreg>>1 )
        if(tempreg[0]) 
            count=count+1;
end
```

**目前：四种循环语句中，综合工具只支持for循环语句**

