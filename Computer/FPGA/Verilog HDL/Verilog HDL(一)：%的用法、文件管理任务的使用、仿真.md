# Verilog HDL(一)：%的用法、文件管理任务的使用、仿真
**1.无符号整数和有符号整数：**  
signed short int –32,768 to 32,767  
signed int –2,147,483,648 to 2,147,483,647  
signed long int –2,147,483,648 to 2,147,483,647  
unsigned short int 0 to 65,535  
unsigned long int 0 to 4,294,967,295

**2.% 用在算数运算中是取模操作符**  
a % b 按照a 和 b中的长度长的补齐。两个参数都为有符号数结果为有符号数，否则为无符号数。  
用在$display语句里面是转意操作符  
%b %B 二进制  
%o %O 八进制  
%d %D 十进制  
%h %H 十六进制  
%e %E %f %F %g %G 实数  
%c %C 字符  
%s %S 字符串  
%v %V 二进制和强度  
%t %T 时间  
%m %M 层次实例

**3.文件管理任务**  
(1)打开文件  
    <file\_handle> =$fopen("<file\_name>")

(2)输出到文件

&nbsp;&nbsp;&nbsp;&nbsp;VerilogHDL中用来将信息输出到文件的系统任务有$fdisplay，$fwrite，$fmonitor。

&nbsp;&nbsp;&nbsp;&nbsp;<task\_name>(<file\_handles>,<format\_specifiers>)

&nbsp;&nbsp;&nbsp;&nbsp;其中：<task\_name> 为：$fdisplay，$fwrite，$fmonitor

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<format\_specifiers>用来指定输出格式

 (3)关闭文件

   $fclose(<file\_handle>)

(4)从文件中读取数据到存储器

   <task\_name>(<file\_name>,<register\_array>,<start>,<end>);

   <task\_name>用来指定系统任务，有：$readmemb、$readmemh  (其中前者以二进制数据格式放入存储器中，后者以十六进制数据格式放入存储器中)

   <file\_name>是读出数据的文件名

   <register\_array> 为要读入数据的存储器

   <start>  ，<end>分别为存储器的起始地址和结束地址

4.仿真控制任务

(1)仿真控制任务

$monitor的用法：$monitor(<format\_specifiers>,signal,signal)      //该任务用来监控指定的信号参数，如果发现其中的任一信号发生变化，则系统按照调用$monitor是规定的格式，在时间结束时显示整个信号表。

$monitoron：打开监控任务

$monitoroff：关闭监控任务

(2)仿真结束任务

Verilog HDL中用来结束仿真的系统任务有$finish和$stop。

$finish用来终止仿真器的运行，结束仿真过程返回到操作系统；

$stop暂时挂起仿真器，进入Verilog界面，可以通过输入相应命令使仿真继续运行。