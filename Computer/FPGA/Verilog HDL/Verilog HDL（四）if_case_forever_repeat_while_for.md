# Verilog HDL（四）if_case_forever_repeat_while_for
### 1.分支语句

**（1）条件分支语句**

```
module sel_from(q,sela,selb,a,b,c);
		input sela,selb,a,b,c;
		output q;
		reg q;
		always @(sela or selb  or a or b or c)
		begin
				if(sela)           q = a;
				else  if (selb)  q = b;
				else              q = c;
		end
endmodule
```

**分析**：如果sela和selb取值都为“1”，即条件表达式“sela”和“selb”都成立，这种情况下由于首先判断条件表达式“sela”,因其成立，所以就执行“q=a”,然后结束整个条件分支，对后面的语句并不做判断。

**（2）case分支控制语句**

```
case(op_code)
2'b00:      out =a&b;   //控制表达式“op_code”取值为2'b00时，执行操作“out =a&b”
2'b01:      out =a|b;
2'b10:      out =a|b;
2'b11:      out =a|b;
default:out = 0;        //op_code和上述分支都不相同时，执行out = 0
endcase
```

casez语句中，如果控制表达式或分支表达式的某一位取值为“z”，在分支语句执行时将忽略该位的比较。  
casex语句中，如果控制表达式或分支表达式的某一位处于为“z”或“x”状态，在分支语句执行时将忽略该位的比较。

### 2.循环控制语句

**（1）forever循环语句**  
forever循环语句实现的时一种无限循环，语句内所指定的循环体部分将不断重复执行  
格式：forever 语句或语句块；

```
initial
begin
		clk = 0;
		#20;
		forever   #5   clk = ~clk;
end
```

该代码为周期为10的波形。

**（2）repeat 循环语句**  
repeat 循环语句是实现一种循环次数预定指定的循环。  
格式：repeat (<循环次数表达式>) 语句或语句块  
循环次数表达式:可以是整数常量、寄存器常量或者一个数值表达式。

例如：用repeat循环语句来实现一个8位乘法器

```
module multiplier_8bits(result,op1,op2);
		parameter WIDTH = 8;         // 参数定义
		parameter LONGWIDTH = 16;
		input [WIDTH-1 :0]	  op1,op2;
		output [LONGWIDTH -1:0]  result;
		reg[LONGWIDTH -1:0] result;
		always @(op1,op2)
		begin :mult
				reg[LONGWIDTH-1:0]  shift_op1,shift_op2;   //局部变量定义 
				shift_op1 = op1;
				shift_op2 =op2;
				result = 0;
				repeat(WIDTH)
				begin    //循环体语句
						if(shift_op2[0])    result  = result + shift_op1;    //相加操作
						shift_op1 = shift_op1<<1;         //操作数移位
						shift_op2 = shift_op2>>1;
				end
		end
endmodule

//若该乘法器中 op1 = 3,op2 = 5，则result = 15
```

**（3）while循环**

while循环实现一种条件循环  
格式：while (<条件表达式>) 语句或语句块；

**（4）for循环**

```
for(<语句1>；<条件表达式>；<语句2>)  循环体语句或语句块
```

等价于：

```
begin 
		<语句1>；
		while（<条件表达式>）
		begin
				循环体语句或语句块；
				<语句2>；
		end
end
```