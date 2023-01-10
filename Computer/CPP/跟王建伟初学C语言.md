
printf ---print format

自带编译：`gcc -o hello hello.c` gcc 苹果 自带编译环境

hello 为生成的可执行文件  
Fhello.c 为自己的源文件

## C语言数据类型

![](vx_images/3874716246521.webp)

  
### 浮点型
浮点型又叫【实型数据】 有两种表示方法：

1.  十进制 比如3.14
2.  指数形式 比如 👇

![](vx_images/594764616243138.webp)

并且在内存中都是以 **指数** 形式存储

**精度**

![](vx_images/590654616257614.webp)

### 字符变量 char

只能存一个字符 ，  
因为一个字符型变量在内存中只占一个字节。  
比如 A 就是 ASCII码`(American Standard Code for Information Interchange)`的 65 ，a 就是 97。

![](vx_images/583564616255054.webp)

**注意** ⚠️  
双引号--字符串 单引号--字符  

 ![](vx_images/579444616236530.webp)

![](vx_images/575324616248696.webp)

### sizeof 运算符（比如 +，-）

![](vx_images/567214616254329.webp)

```cpp
#include <stdio.h>
#include <stdlib.h>

int main(){
    printf("hello🇳🇱\n");
    int abc = 012; //八进制
    printf("012的十进制数是：%d", abc); //10
    abc = 0x12; //十六进制
    printf("012的十进制数是：%d", abc); //18
    printf("abc变量占用的内存字节数是：%d", sizeof(abc));  //4

    char d[]="abcdef" //sizeof(d) = 7; strlen(d) = 6;
    char* c = "abcdef" //sizeof(c) = 4指针都是4，strlen(c) = 6
}
```


## 运算符

### 关系运算符

![](vx_images/436864516261215.webp)

### 逻辑运算符

！ && ||

### 优先级：

![](vx_images/563094616239866.webp)

![](vx_images/550004616262317.webp)

![](vx_images/546904616239017.webp)

从右到左

![](vx_images/543804616245053.webp)

![](vx_images/539704616240491.webp)

逗号表达式：

![](vx_images/537464616257861.webp)

![](vx_images/533364616242395.webp)

## 语句
### 语句分类:

 任何表达式有分号，就成了语句

1.  控制语句

![](vx_images/526274616245565.webp=320*200)

2.  函数调用语句

```bash
printf("hallo");
```

3.  表达式语句  
    3+5;
    
4.  复合语句  
    if else { } 大括号不加 分号 ；
    

### include

`#include <stdio.h>` 失去系统目录中找文件。标准的头文件用 <>  
`#include "stdio.h"` 先在当前目录查找, 如果找不到再找系统中找。  
自己写的文件 📃 用 " " 双引号比较好

### putchar()只输出一个

printf 就是这个只是打印一个  
%d 打印十进制  
%0 打印八进制  
%x 打印十六进制  
%u 打印unsigned型数据  
%c 打印一个字符 （0-255之间的数字）

![](vx_images/503264616241445.webp)

%s 打印字符串  
%f 打印实数

![](vx_images/491174616248115.webp)

打印 5%

### 输入

![](vx_images/483064616260299.webp)

输入时字符串时，比如 “how are you” ，只会输出 how，遇到空格丢弃后面。

```cpp
    int i,j,k;
    scanf("%d,%d,%d", &i, &j, &k); //必须输入时也打 逗号
    printf("i+j+k= %d", i+j+k);

    scanf("%d,%d,%d", &i, &j, &k); //直接按回车，空格 tab即可
```

### switch

> 不要忘记**break**；  
> 不写的话会 比如下面例子：进入case5那儿打印完继续往下走到 default 里面，直到遇到 break 或者程序走完停。  
> 如果abc = 0，走第一个 打印 0，再进入 case1 打印 1 然后 break 出来；没有 break 就一直往下走

defined 不是必须；

```cpp
int main(){
    int abc = 5;
    switch( abc ){
        case 0:
            printf("0");
            break; 
        case 1:
        {
            printf("1"); //可写括号或不写都行
            break;
        }
        case 2:
            printf("2"); //
            printf("还可以写几行");
            printf("加不加括号都行");
            break;
        case 3:
        case 4:
        case 5:
            printf("可以多个条件都满足\n");
            printf("3\n");
            printf("4或者5");
            break;
        default:
            printf("default可以不写");
            break;
    }

}
```



### 循环控制语句
包括 goto, while, do while, for

### goto语句：

无条件转向语句，用来跳转到某个程序位置进行执行。

goto 语句标号： 比如  
goto 123；不行 🚫 ！第一个不能为数字

🌰：从一到100的加法:

```cpp
int main(){
    int i = 1, sum = 0;

loop:  //必须在同一个函数里，这里是 main
    if(i<=100){
        sum += i;
        i++;
        goto loop;
    }
    printf("sum的值为：%d\n",sum);//5050
    printf("i的值为：%d",i);//101
}
```

很多情况下，可以用其他循环语句代替 goto，不常用。  
**goto 不能有 break 和 continue**

```cpp
    while(i<=100){
        sum += i;
        i++;
    }
        printf("i的值为：%d",i);//101
        printf("sum的值为：%d\n",sum);//5050
```

![](vx_images/432694516233538.webp)

![](vx_images/427594516244074.webp)

![](vx_images/420494516251832.webp)

此时 i = 100，不是 101。i 必须 > 且 = 100

![](vx_images/415404516247049.webp)

![](vx_images/410304516231258.webp)

![](vx_images/403184516257458.webp)

![](vx_images/399084516242160.webp)

一共打印次数： 1 + 2 + ... + 9 = 45 次

### continue : 不执行后面的程序，重新循环

![](vx_images/394994516234139.webp)

## 数组

![](vx_images/390904516245682.webp)

![](vx_images/384784516253763.webp)

### 二维数组

![](vx_images/379644516254875.webp)

内存中，是按照一行完了存下一行来存储的

### 字符数组

引入头文件 `#include <string.h>`

![](vx_images/374514516252463.webp)

转义字符 \\0

![](vx_images/368374516248744.webp)

![](vx_images/362274516239269.webp)

如果数组本来长12，只填满了9个字符，  
那 `第十 c[10] = \0 增加一个结束符号`;  
`c[11] = 0 用0填满`

![](vx_images/358174516243398.webp)

![](vx_images/355064516254094.webp)

字符数组名 本身代表该数组的起始地址

```rust
char str[10];
printf("%s", str) //等同于下面
printf("%s", &str) 
```

但是 int 类型就不是同一回事

```cpp
int i;
printf("%d", i) //打印 i 的值
printf("%d", &i) //打印 i 所在内存的地址值
```

## 字符串处理函数

字符串就是字符的一维数组

1.  puts

![](vx_images/349984516246620.webp)

2.  gets

![](vx_images/343894516235205.webp)

3.  strcat （`重点` ）

![](vx_images/338774516245681.webp)

![](vx_images/333674516256885.webp)

4.  strcpy (`重点`)

![](vx_images/328564516235937.webp)

![](vx_images/323434516236378.webp)

字符数组 1 必须是个数组名，字符串2可以试数组名，也可以是字符串常量，比如：

```cpp
char str1[10] = "str1";
strcpy( str1, "str2");//因为考到一个地方，得有地址值，str1数组名就是地址值
```

![](vx_images/317314516249180.webp)

5.  strcmp

![](vx_images/311184516257032.webp)

![](vx_images/305094516258853.webp)

6.  strlen (`重点`)

![](vx_images/299014516235129.webp)

## 函数

函数分类：库函数，自定义函数。  
main函数是留给系统调用。

一个文件里有一个或者多个函数组成，这个文件一般就成为【源程序文件】。

无返回类型的函数，必须写 `void`;  
形参在函数调用之前不分配内存，调用的时候分配内存，函数调用结束后，形参的内存被释放，所以形参只能在函数内部使用；

![](vx_images/292914516252494.webp)

return 只能返回一个值；

![](vx_images/287804516261781.webp)

### 函数声明

函数声明就是把函数定义去掉大括号，加分号。（可以提前指明）  
把函数声明一般放在【源代码的开头】。因为 `main`函数要调用别的函数，必须先有别的函数的声明在前面

```cpp
sum(); //函数声明
int main(){
  sum(); //这样才能调用 sum 函数
}
sum(){  //函数定义
}
```

### 递归

递归必须有一个出口（结束条件），不然会崩溃

![](vx_images/277694516232414.webp)

🌰 计算 5 的阶乘

```cpp
#include <stdio.h>
#include <stdlib.h>

int recursion(int n); //不声明会有警告 ⚠️
int main()
{
    int sum = recursion(5);
    printf("sum = %d ", sum);
}

int recursion(int n)
{
    int result;
    if (n == 1)
    {
        return 1; //出口
    }
    else
    {
        result = recursion(n - 1) * n;
    }
    return result;
}
```

![](vx_images/271604516244085.webp)

### 数组名作为参数 ----- 地址传值！！！

传递的是【地址值】，所以实参形参都指向同一内存地址。  
任何一方发生变化，另一方跟着变。

![](vx_images/268494516240038.webp)

b为形参，a为实参

![](vx_images/264384516252812.webp)

b 为 int，必须跟实参 int a 一致才行，不然内存里面存的地址就会不一样

```cpp
void changevalue(int b[]);  // b 的长度无关，写 b[1000]都行
int main()
{
    int a[5] = {1, 2, 3, 4, 5};
    changevalue(a);
    for (int i = 0; i < 5; i++)
    {
        printf("a[%d] = %d\n", i, a[i]); //此刻再打印 a[5] 里边就被改变了
    }
}
void changevalue(int b[])
{
    b[1] = 0;
    b[2] = 0;
    return;
}
```

形参 b\[\] 不会被 c 编译器检查大小，所以随便写多大。

### 多维数组作为参数

和上边一样。

> 注意 ⚠️ ：  
> 形参第一维下标可以省略，但二维不能省略！

```cpp
void changevalue(int b[][9]);  // 第二维不能省略！
int main()
{
    int a[8][9]; // 不能a[8][8] 拿不到 [8]
    a[5][8]= 2;
    changevalue(a);
    printf("a[5][8] = %d\n", a[5][8]); //此刻再打印 a[5][8] 元素就被改变了
}
void changevalue(int b[][9])
{
    b[5][8] = 0;
    return;
}
```

所以： 最好 a\[8\]\[9\] ，b 作为参数也写成 b\[8\]\[9\] 就不容易出错！

## 全局/局部变量

**全局变量缺点：**

1.  全局变量一直在整个周期之间都占用内存；
2.  迁移函数到另一个文件，万一另一个函数也有该全局变量名，引起冲突；函数忘了迁移走一个用到的全局变量；
3.  一个函数改了一个全局变量的值，又在其他函数中用这个全局变量，但已经很难发现被改动了，导致程序的可读性和清晰性变差；

![](vx_images/261294516259766.webp)

不放在最前面当全局变量，就得加上关键字 extern

![](vx_images/255194516234747.webp)

全局变量和局部变量重名时

```cpp
int a = 10, b = 20;
void look(int a, int b) //虽然和全局变量重名，但也是局部变量
{                       //在局部变量作用范围内，全局变量不起作用
    a = 100;
    b = 200;
    return; //影响不到全局 a，b
}
int main()
{
    a = 1, b = 2;
    printf("a=%d, b=%d\n", a, b); //1, 2
    look(a, b);                   //1, 2 进入函数内 返回 a = 100， b = 200（但是是局部变量，调完就销毁）
    printf("a=%d, b=%d\n", a, b); //1，2 没有影响
}
```

如果 main 就用外面的全局变量，look()函数一样影响不到

```cpp
int a = 10, b = 20;
void look(int a, int b) //虽然和全局变量重名，但也是局部变量
{                       //在局部变量作用范围内，全局变量不起作用
    a = 100;
    b = 200;
    return; //影响不到全局 a，b
}
int main()
{
    // a = 1000, b = 2000;
    printf("a=%d, b=%d\n", a, b); //10, 20
    look(a, b);                   //10, 20 进入函数内 返回 a = 100， b = 200（但是是局部变量，调完就销毁）
    printf("a=%d, b=%d\n", a, b); //10，20 没有影响
}
```

***

### 变量的存储类别

从变量存在的时间（生存期）角度来划分：

1.  静态存储变量
2.  动态存储变量

**静态存储方式：**在程序运行期间分配固定存储空间的变量。这种分配变量的方式就叫做静态存储方式。  
**动态存储变量：**在程序运行期间根据`需求`进行动态分配存储空间的变量。这种分配变量的方式就叫做动态存储方式。

![](vx_images/250104516261223.webp)

静态就是stack栈，动态就是heap堆

## 局部变量的存储方式

1.  传统情形：函数调用时分配内存，执行完释放空间。
2.  特殊情形：  
    a）局部静态变量：  
    用 **static** 加以说明。

```cpp
void test()
{                          传统的留不住值，三次调用三次销毁
    int c = 4;
    c++;
    printf("%d\n", c);
    return;
}
int main()
{
    test();//5
    test();//5
    test();//5
}
```

局部静态变量：存储在静态存储区域

```cpp
void test()
{                        只多了一个 static 在最前面
    static int c = 4;      这个 4 在被调用之前就已经分配好了内存
    c++;                    下一次调用就是上一次的值，不被释放
    printf("%d\n", c);        运行 test() 的时候根本不会去执行 static 这行
    return;
}
int main()
{
    test(); //5
    test(); //6
    test(); //7
}
```

如果 `static int c` 没有赋值，默认为 0，且只能在它**自己的函数**内用

## 全局变量不被其他 project 引用

`project1.cpp` 的函数要用在 `project2.cpp`，就要在 2 里面的最上面加上

![](vx_images/242004516234941.webp)

![](vx_images/236904516246649.webp)

## 外部函数的引用

加上 `static` 成为静态函数，就只能在本project.cpp里面用。  
如果`project1.cpp`和`project2.cpp`都有同一名称函数，就会报错，除非两个都是`static`，就没事儿，因为只能在自己区域内使用。

### static 总结

![](vx_images/227794516253037.webp)

### 编译预处理

![](vx_images/220674516232190.webp)

#### 宏定义

> 程序目的：一个项目可以通过编译（源文件），链接最终成为一个可执行文件。

每个源文件`.cpp`都会单独编译成一个目标文件`（.o, 或者.obj，扩展名跟操作系统有关obj是Windows)`。  
系统把 `.o` 文件进行链接，最终形成**一个**可执行文件。

**编译**\-----词法，语法分析，目标文件临时的生成，优化。  
分为以下几步：

1.  预处理；  
    .cpp里面加入一些特殊代码（特殊命令），编译系统会先对这些特殊代码做处理，这个处理就叫 【预处理】。（结果再和源程序一起下两步。）
    
2.  编译：词法，语法分析，目标代码生成，优化；
    
3.  汇编：产生 .o/.obj 目标文件；
    


**预处理** 又分为三种处理功能：  
a. 宏定义  
b. 文件包含  
c. 条件编译  
这三种功能也是通过在程序代码中写代码来实现，只不过这些代码比较特殊，是以 # 开头。

### 不带参数的宏定义

> 定义：用一个指定的标识符，代表一个字符串。  
> 格式：#define 标识符（宏名） 字符串

![](vx_images/213584516246226.webp)

![](vx_images/201474516251646.webp)

**说明：**  
i. 宏名一般用大写字母。  
ii. 末尾不要加分号。  
iii. #define 写最上面，作用域是整个当前文件📃。  
iiii. #undef 可以终止宏定义作用域

![](vx_images/195364516235645.webp)

  

v. 用#define进行宏定义时，还可以引用已经定义的宏。（层层替换）

![](vx_images/186204516241431.webp)

![](vx_images/180094516254734.webp)

vi.

![](vx_images/167004516262547.webp)

### 带参数的宏定义

![](vx_images/157244516247052.webp)

![](vx_images/149044516256416.webp)

**宏**和**函数**的区别：

![](vx_images/143924516244039.webp)

***

#### 文件包含

![](vx_images/103514416237484.webp)

一般引用 h 文件📃（头文件）；  
而常常把宏定义，函数说明，以及其他一些全局变量的外部声明等等放在头文件中。

***

### 条件编译

![](vx_images/96414416235074.webp)

第二种写法：

![](vx_images/92324416241948.webp)

第三种写法：

![](vx_images/86224416248864.webp)

***

### 指针

有些内存是在编译分配的；  
有些是在程勋运行时分配的；

int, float, double, char...**变量**都是会占用一段内存空间。  
int -- 4 字节 （sizeof）  
double -- 8 字节

### 地址：

计算机中，用一个 `16进制`（0xXXX） 数字 🔢 来描述一个地址。  
【数字】 代表 【位置】

> 注意 ⚠️ ：  
> 严格区分 `地址` 和 `地址中内容` 的区别

![](vx_images/79134416250062.webp)

  
内存里不知道存了 `i` 和 `j` ， 但是程序内部会一直维持着一张表：

> **1000 --- i** （int）系统知道要取 `4` 节才是这个 i  
> **1004 --- j** 当 printf(i) 系统就去第`1000`格取`4`节，拿到`5`

### 直接访问和间接访问

**直接访问：**上面👆取 `i` 这种。  
常规的 int，char，float，double 都是拿来存变量的“值”，比如上面👆int i = 5；  
**间接访问：**将变量`i`的地址，存放在另一个内存单元中。通过指针变量来读取。

> **指针变量:** 在 `C语言`中， 定义一种**特殊**的变量，这种特殊的变量用来保存**地址**。  
> 指针变量的【值】就是别人的【地址】，也叫做【指针】。  
> （指针就是地址！！！）  
> 它也分保存 哪种(int) 类型的地址。这样才能够知道取几格来拿值。

比如 🌰：  
`mypoint` 就是存放整形变量的地址，(上面`i`就是存放的整形 `5`)

```cpp
//把变量` i `的地址保存到了`mypoint `里面，
//所以 `mypoint` 指向了 `i` 。（指向通过地址体现）
int *pMyPoint;     定义时，有 * 星号
mypoint = &i;      使用时没有，指针变量名是 mypoint

int *mypoint = &i;    
```

并且，`mypoint` 它是个变量，虽然特殊，但它还是要占用内存( 4个字节 )，所以它本身也是有地址的！

![](vx_images/72044416254192.webp)

【间接访问】

### 变量的指针 & 指向变量的指针变量

![](vx_images/63934416242731.webp)

  

查看内存的方法

![](vx_images/58834416249737.webp)

  
内存里面`i`的样子

![](vx_images/49674416233656.webp)

#### 指针变量的引用

指针变量中，只存放地址。  
**&：取地址运算符**  
\***： 指针运算符**

![](vx_images/37594416254501.webp)

![](vx_images/31494416261209.webp)

![](vx_images/23404416245114.webp)

![](vx_images/18314416245458.webp)

```cpp
int main()
{
    int *pmax, *pmin, *p, a, b;
    a = 5;
    b = 8;
    pmax = &a;
    pmin = &b;
    if (a < b)
    {
        p = pmin;    //p 指向 b
        pmax = p;    //pmax 也指向 b
        pmin = pmax; //pmin 也指向 a
    }
    printf("a=%d, b=%d\n", a, b); //a=5, b=8
    printf("max=%d, min=%d", *pmax, *pmin);//max=8, min=8
}
```

![](vx_images/13234416254478.webp)

### 函数的参数是指针类型

作用：将一个变量的地址传送到一个函数中去。

```cpp
void swap(int *pdest1, int *pdest2)
{
    int temp;      交换指针没有用，要交换值才行！
    temp = *pdest1;    // temp = a => temp = 5
    *pdest1 = *pdest2; // a = b => a = 8
    *pdest2 = temp;    // b = temp => b = 5
}
int main()
{
    int *pa, *pb, a, b;
    a = 5;
    b = 8;
    pa = &a;
    pb = &b;

    printf("a=%d, b=%d\n", a, b);
    if (a < b)
    {
        swap(pa, pb);
    }
    printf("a=%d, b=%d\n", a, b); //a=8, b=5
}
```

![](vx_images/7134416236837.webp)

**指针赋值：**  
int \*p1, \*p2;  
p1 = p2; // 就是我指向谁，你也跟着我指向谁

### 数组的指针

定义：数组的开始地址。  
数组元素的指针：数组元素的地址。

```cpp
int a[5] ;
a[0] = 5; a[1] = 6; a[2] = 7; a[3] = 8; a[4] = 9;
int *p = &a[0]; //这行和下面等价 
p = a; //等于上面
```

![](vx_images/1024416245998.webp)

通过指针引用数组元素

![](vx_images/594934316253913.webp)

![](vx_images/589824316257554.webp)

p+4 ---> 1016 应该

```cpp
int main()
{
    int a[] = {5, 6, 7, 8, 9};
    int *p;
    p = a; // p = &a[0];
    p[2] = 15;
    *p = 19; // a[0] = 19;
    p = a + 3;
    *p = 14;       // a[3] = 14;
    *(p + 1) = 19; // a[4] = 19;

    for (int i = 0; i < 5; i++)
        printf("a[%d] = %d\n", i, a[i]);  //*(a+i)
    // a[0] = 19
    // a[1] = 6
    // a[2] = 15
    // a[3] = 14
    // a[4] = 19
    for (p = a; p < (a + 5); p++)  //这样是效率最高的循环，不用算地址
        printf("%d\n", *p); //也打印上面的值  a[0] - a[4]
}
```

![](vx_images/581734316252756.webp)

![](vx_images/575654316249372.webp)

```cpp
int main()
{
    int a[5] = {5, 6, 7, 8, 9};
    int *p;
    p = a; // p = &a[0];
    // printf("%d\n", *++p); //a[1] -> 6 然后p指向了a[1]
    printf("%d\n", *p++); //a[0] -> 5 然后p指向了a[1]
}
```

![](vx_images/568544316239050.webp)

```cpp
int main()
{
    int a[5] = {5, 6, 7, 8, 9};
    int *p;
    p = a;                // p = &a[0];
    (*p)++;               // a[0]++;
    printf("%d\n", a[0]); //6
    p++;                  //p指向了a[1]
    (*p)++;               // a[1]++;
    printf("%d\n", a[1]); //7
}
```

### 数组名作为函数参数

用指针变量来改变数组的值

```cpp
void changevalue(int *p) //之前写过 int b[]  b[1] = 3;
{
    *(p + 1) = 3; // a[1] = 3
    p[2] = 0;     // b[2] = 0
}

int main()
{
    int a[5] = {5, 6, 7, 8, 9};
    //changevalue(a);                              实参用数组名
    int *p = a;
    changevalue(p);                                //实参用指针
    printf("%d\n", a[1]); //3
    printf("%d\n", a[2]); //0
}
```

### 多维数组的指针

![](vx_images/562454316239148.webp)

![](vx_images/554354316260214.webp)

```cpp
int main()
{
    int a[3][4];
    for (int i = 0; i < 3; i++)
    {
        for (int j = 0; j < 4; j++)
        {
            a[i][j] = 0;
        }
    }

    int *p;
    //改变一行一列的元素
    p = &a[1][1];
    *p = 1;

    for (int i = 0; i < 3; i++)
    {
        for (int j = 0; j < 4; j++)
        {
            printf("a[%d][%d]=%d ", i, j, a[i][j]);
        }
        printf("\n");
    }
}
```

![](vx_images/540204316230396.webp)

  
可通过 `p++` 或 `p--`来调整选择下一个还是上一个。

```cpp
int main()
{
    int a[3][4];
    for (int i = 0; i < 3; i++)
    {
        for (int j = 0; j < 4; j++)
        {
            a[i][j] = 0;
        }
    }

    int *p;
    //强制类型转换
    p = (int *)(a + 1); // 第一行首地址 a[1][0]...a[1][3]
    *p = 1;
    p++; //横着走四个字节 a[1][1]
    *p = 2;

    for (int i = 0; i < 3; i++)
    {
        for (int j = 0; j < 4; j++)
        {
            printf("a[%d][%d]=%d ", i, j, a[i][j]);
        }
        printf("\n");
    }
}
```

![](vx_images/536134316231950.webp)

#### 指针数组 & 数组指针

### 指针数组

![](vx_images/527044316252298.webp)

![](vx_images/518934316233143.webp)

### 数组指针

用得不多，不好理解，能掌握就掌握，至少熟悉基本概念。

![](vx_images/511844316242424.webp)

一个指针变量，用来指向含有10个元素的一维数组。

```cpp
int main()
{
    int a[10];
    int(*p)[10];

    for (int i = 0; i < 10; i++)
    {
        a[i] = i;
    }
    p = &a; //p = a 会有一个警告，其实 a 和 &a 内存地址一样

    int *q;
    q = (int *)p;

    for (int i = 0; i < 10; i++)
    {
        printf("value = %d\n", *q);  //遍历数组
        q++;
    }
    
    //不通过 q 会有一个警告 说 打印 %D 是int，但给的参数是 int *
    for (int i = 0; i < 10; i++)  
    {
        printf("value = %d\n", *p);
        p++;
    }
}
```

![](vx_images/503754316230886.webp)

二维情况下：

```cpp
int main()
{
    int a[3][10];
    int(*p)[10];

    for (int i = 0; i < 3; i++)
    {
        for (int j = 0; j < 10; j++)
            a[i][j] = i + j;
    }
    p = a; //p = &a 反而会有一个警告，二维数组名可以直接赋值给数组指针

    int *q;
    q = (int *)p;

    for (int i = 0; i < 3; i++)
    {
        for (int j = 0; j < 10; j++)
        {
            printf("%d   ", *q);  //*(*(p + i) + j) 也行
            q++;
        }
        printf("\n");
    }
}
```

![](vx_images/499654316257637.webp)

![](vx_images/494544316239849.webp)

![](vx_images/488444316238831.webp)

***

## 字符串指针

![](vx_images/485354316242967.webp)

![](vx_images/473244316250145.webp)

一段字符串数组拷贝给另一个数组

```cpp
int main()
{
    char a[] = "I love China!";
    char b[100];                       //保证比a字符数组长
    int i;                             //必须把 i 写外面不然 *(b+i) 拿不到 i
    for (i = 0; *(a + i) != '\0'; i++) //*a(a+i)相当于a[i]
    {
        *(b + i) = *(a + i); //b[i] = a[i]
    }
    *(b + i) = '\0'; //b[i] = '\0';
    printf("%s", a);
    printf("%s", b);
}
```

![](vx_images/464144316262865.webp)

另一正更好理解的方式：`p1++`, `p2++`

```cpp
int main()
{
    char a[] = "I love China!";
    char b[100]; //保证比a字符数组长
    char *p1, *p2;
    p1 = a;
    p2 = b;
    for (; *p1 != '\0'; p1++, p2++) //刚开始循环 p1 = &a[0]，最后 p1 = &a[i] 在最后一位
    {
        *p2 = *p1; //b[i] = a[i]
    }
    *p2 = '\0'; //b[i] = '\0';此刻在指针指向最后一位
    printf("%s", a);
    printf("%s", b);
}
```

### 字符串指针做函数参数

以下几种都可以：

```csharp
void copeValue(char from[], char to[]) //char *from, char *to
{
    int i = 0;
    while (from[i] != '\0')
    {
        to[i] = from[i]; //逐个字符拷贝
        i++;
    }
    to[i] = '\0';

        操作 指针 比 下标[] 效率更快，不需要去转换[] 元素的地址
-----------------------------------------------------------------------------
    for (; *from != '\0'; from++, to++)
    {
        *to = *from; // b[i] = a[i]
    }
    *to = '\0';
-----------------------------------------------------------------------------
        while (*from) //直到遇到 '\0' 就是 0 (ASCII编码)
    {
         // *(to++) 先加后用，相当于先把 a[0] 的值 给 b[0]，
         //然后移动一个字节指针到第二个元素
        *to++ = *from++; 
    }
    to[i] = '\0';
}
int main()
{
    char a[] = "I love China!";
    char b[] = "I love China too!"; //保证 b 比 a 长
    char *p1, *p2;
    p1 = a;
    p2 = b;

    printf("%s\n", a);
    printf("%s\n", b);
    copeValue(a, b);
    printf("%s\n", a);
    printf("%s\n", b);
}
```

注意⚠️ ：

```rust
① char str[100] = "xxxxxx"   定义初始化，相当于【拷贝】字符串内容到str中

不能分成两部分写！
char str[100];
str = “xxxxx” 这是一个 const 常量了，报错👇

只能 【拷贝】的方式：
② strcpy(str, "xxxxx");              ① 就是 ②
```

![](vx_images/457044316262770.webp)

![](vx_images/451954316257752.webp)

***

### 函数指针

### 函数指针变量调用函数

**定义：**一个函数在编译的时候，系统会给这个函数分配一个入口地址，这个入口地址，就成为`函数的指针`。  
所以我们可以定义一个指针z变量指向该函数，来调用该函数。

每个函数在可执行文件执行时，都会占用一段内存单元，这些函数有起始地址，就可以用一个指针变量指向该函数，从而调用该函数。

![](vx_images/442844316255655.webp)

![](vx_images/433744316236976.webp)

### 用指针当参数

```cpp
int max(int *p1, int *p2)
// int max(int a, int b) //int a, int b;
{
    return *p1 > *p2 ? *p1 : *p2;
    // return a > b ? a : b;
}

int main()
{
    int a = 0;
    int b = 1;
    int *p1, *p2;
    p1 = &a;
    p2 = &b;
    int (*p)(int *, int *); 
    p = max; 
 
    int z = (*p)(p1, p2); 
    // int x = max(p1, p2); // max(a,b);
    
    // printf("this is the bigger number: %d\n", x);
    printf("this is the bigger number: %d\n", z);
}
```

指针作为参数传递

```cpp
int max(int x, int y)
{
    return x > y ? x : y;
}
int middleMax(int x, int y, int (*p)(int, int))
{
    return p(x, y); //调用 max
}
int main()
{
    int res = middleMax(0, 1, max);
    printf("this is the bigger number: %d\n", res);
}
```

```cpp
int max(int x, int y)
{
    return x > y ? x : y;
}
  如果还有更多函数，都可以通过这个函数来调用，很灵活
int middle(int x, int y, int z(int, int))
{
    return z(x, y); //调用 max
}
int main()
{
    int res = middle(0, 1, max);
    printf("this is the bigger number: %d\n", res);
}

```

### 返回指针值的函数

返回一个地址；（不常用）

```cpp
int sum;
int* add(int x, int y) //返回值时一个指针 int*
{
    sum = x + y;
    return &sum;
}
int main()
{
    int *res;
    res = add(0, 1); //返回一个指针就要用一个指针去接
    printf("sum = %d\n", *res);//sum = 1
    printf("sum的地址 %p\n", res);//sum的地址 0x10a366018
}
```

## 

![](vx_images/425654316258361.webp)

![](vx_images/415554316237819.webp)

***

### 指针的指针，指针数组

### 指针数组

![](vx_images/365454216245249.webp)

```cpp
int main()
{
 char* pName[] = {"C++", "JAVA", "PYTHON", "GO", "CSharp"};
 int is1 = sizeof(pName); //
 int isize = is1 / sizeof(pName[0]);//
 int i = 0;
 char* p = "JAVA";//取头一个

 for (i = 0; i < isize; i++) {
 
     其实每一个 pName[0]...都指向一个首字母，but
     printf 内在有一种机制会自动把一个【字符串指针】所指向的 字符串 打完
    //  printf("pName[%d] = '%s'\n", i, pName[i]);  

    char* q = pName[i];
     while (*q) {
     printf("%c", *q);    
     q++;
    }
    printf("\n");
 }

 printf("p指向%c\n", *p); //p指向J

 同理这个地方也是一个 p 指针，可以 printf 一个 string！！！！！ 
 printf("p指向的整个内存%s\n", p); //p指向的整个内存JAVA
 
 printf("--------------------------------\n");
 
 char* p3;
 p3 = pName[0];
 pName[0] = pName[1];
 pName[1] = p3;

 for (i = 0; i < isize; i++) {
     printf("pName[%d] = '%s'\n", i, pName[i]);
 }
 printf("p指向%c\n", *p);//p指向J
 printf("p指向的整个内存%s\n", p); //p指向的整个内存JAVA
}

```

> **特别注意 ⚠️ ：**  
> for (i = 0; i < isize; i++) {  
> printf("pName\[%d\] = '%s'\\n", i, pName\[i\]);  
> }  
> 同理这个地方也是一个 p 指针，可以 printf 一个 string！！！！！  
> 用的 `%s` 和 `p`  
> printf("p指向的整个内存%s\\n", p); //p指向的整个内存JAVA

### 指向指针的变量----指向指针的指针

```cpp
int main()
{
 char* pName[] = {"C++", "JAVA", "PYTHON", "GO", "CSharp"};

 char* p = "JAVA";// p 指向 JAVA 的首地址 J
 printf("%c\n", *p); // J
 p++;
 printf("%c\n", *p); // A

 char** pp1;
 pp1 = &pName[0];
 printf("%s\n",*pp1);// C++

 char** pp2 = &pName[1];
 printf("%s\n",*pp2);// JAVA

 int abc = 5;
 int* pabc = &abc;
 int** ppabc = &pabc;
 printf("abc = %d\n", abc); //5
 printf("*pabc = %d\n", *pabc); //5
 printf("**pabc = %d\n", **ppabc); //5
}
```

### 指针数组做 main 函数参数

有两种方法。  
一. 在`编辑器`里面添加

![](vx_images/302364216234772.webp)

第一步

![](vx_images/296244216247781.webp)

第二步

![](vx_images/285144216260441.webp)

![](vx_images/268944216261430.webp)

二. 在 `terminal/CMD` 里面添加

![](vx_images/262854216253919.webp)

![](vx_images/253754216258811.webp)

#### 指针总结

![](vx_images/245604216240002.webp)

![](vx_images/227514216247143.webp)

![](vx_images/221424216256407.webp)

***

## 结构体

是一种**数据类型**，就像 `int`。  
**定义：**把多种不同类型的数据放在一起。  
**目的：**是表达更丰富的信息。

**使用**结构体分两步：  
① 一般先定义一个结构体类型，②然后定义某些变量。

**特点：**结构体内可以嵌套结构体。  
结构体内的成员名（`num`，`year`）可以与程序里的变量名相同，互不影响。

**用法:**三种。`第一种`相对用得多点。

```cpp
struct student //最好写到 main 函数之外
{
    int num;
    char name[100];
    int sex;
    int age;
    char address[100];
    struct date birthday; //嵌套另一个结构体
}; // 不要忘了 分号 ；
int main(int argc, char *argv[]) //整型，字符指针数组
{
    struct student s1, s2; // 第一种定义方法：定义两个结构体类型变量
    int num;// 与 student 里面的 num 互不影响
}
struct date // 第三种定义方法：student 都可以省去 直接用 s1，s2（除非如果后续不再用 结构体名 student）
{
    int date;
    int month;
    int year;
}w1,w2;  // 第二种定义方法：定义结构体的同时定义了两个结构体类型变量（变量列表）
```

![](vx_images/213304216255775.webp)

struct student 学生结构体

### 结构体类型变量的引用：

1.  不能将结构体变量作为一个整体进行引用，比如`s1`，`s2`都是结构体变量，但不能拿过来直接用，只能对结构体变量中的各个成员分别引用，引用的方式为：`s1.name`。  
    这个 点 "." 叫做结构成员运算符和（）平级，优先级非常高，直接整体看待。
    
2.  如果成员本身有属于一个结构体类型，则要用若干个个成员运算符。  
    比如`s1.birthday.month = 12`。
    
3.  成员变量，就当成普通变量，可以进行各种运算。  
    比如`s1.age = s2.age; int total = s1.age + s2.age；s1.age++`。
    
4.  因为成员变量也是普通变量，所以他们也是有地址的。  
    `int *p = &s1.num; printf("%d", *p);`
    

### 结构体类型变量的初始化：

1.  整个一起直接初始化：

![](vx_images/205214216250020.webp)

2.  一个个：

```bash
s1.age = 18;
s1.birthday.month = 12;
s1.name = "张三";
```

### 结构体数组

比如有 10 个 student 结构体。  
第一种方法：

```css
struct student stu[2]; 
stu[0]；第一个
stu[1]; 第二个
```

![](vx_images/189104216231977.webp)

第二种方法：

```cpp
struct student 
{
    int num;
    char name[100];
    int sex;
    int age;
    char address[100];
    struct date birthday; //嵌套另一个结构体
} stu[2]；   这儿来两个
```

### 结构体指针

结构体变量的指针，指向该结构体变量所占据的内存段的**起始**地址。  
也可以指向结构体数组中的元素，因为结构体数组中的每个元素它就相当于一个结构体变量。

![](vx_images/178984216241646.webp)

![](vx_images/169894216244879.webp)

![](vx_images/159794216251834.webp)

打印李四的学号，并且指向李四 。注意⚠️ ： ++ps（先加后用） 和 ps++（先用后加） 的区别

![](vx_images/153674216235555.webp)

### 结构体指针 作为 函数参数

```cpp
struct date 
{
    int date;
    int month;
    int year;
};
struct student 
{
    int num;
    char name[100];
    int sex;
    int age;
    char address[100];
    struct date birthday; 
}; 
void changeAge(struct student* pd) //不要写成 int* 而且函数要写在struct student 的后面！！
{
    pd->age = 100;
}
int main() 
{
    struct student stu[]= {
        {1001,"张三",1,13,"1栋1单元",15,10,2000},
        {1002,"李四",1,20,"2栋2单元",1,12,1999}
    };
    // struct student* ps = &stu[0];    等价于下面这个
    // struct student* ps = stu; //操作张三
    struct student* ps = &stu[1];  // 操作李四
    changeAge(ps);
    printf("%s's age is %d\n", ps->name, ps->age);//李四's age is 100
}
```

> 注意 ⚠️ ：这种情况值不会变，【实参】和【形参】的地址不一样。是不是因为这种参数的类型不是primitive？

![](vx_images/145554216250003.webp)

  

相当于 sd 对 张三 或 李四 做了一次内存的拷贝！开销很大，费时间和空间。影响程序运行效率，如果一个结构体很大，就要开很大内存空间进行赋值，而只传指针一样可以实现改变原结构体。

![](vx_images/135464216249826.webp)

所以：建议用指针作为参数更好。

***

## 共同体（联合）

**定义：**把几种不同的类型的变量存放在同一段内存单元（同一个内存地址开始的单元中）。  
内容肯定会相互覆盖。

### 结构体和共用体完全不一样：

结构体占的内存是所有结构体变量内存的 【和】。  
共用体站的内存长度 = 最长的变量长度。

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
union muUni 
{
    int carNum;
    char carType;
    char carName[60];
}a,b,c; //第一种方法定义共用体变量

int main() 
{
  union muUni d,e,f; //第二种方法定义共用体变量
  int length = sizeof(union muUni);
  printf("%d\n", length); // 60 最长的变量 carName[60]

  a.carNum = 1289234898; //引用
  strcpy(a.carName,"ab"); 
  printf("%s\n",a.carName);
  printf("%d\n",a.carNum);   还是可以打印出来，打印的时候又马上替换了
}
```

> 同一段内存存放几种不同类型的成员，**但是** `每个瞬间只存放其中一种`。

![](vx_images/125374216248824.webp)

### 共用体变量的地址 和 其成员的地址都相同；

&a, &a.carNum, &a.carType, &a.carName

### 共用体不能在定义的时候初始化！

![](vx_images/117274216247529.webp)

### 不能把共用体变量，作为函数参数，也不能让函数带回共用体变量

***

### 枚举类型

产生原因：用0，1，2，3 表示颜色，没有直接用 Red， Green， Blue， Yellow 来得醒目。

变量的值只限于列出来的这些值得范围内。

```cpp
enum color
{
    Red,  //枚举常量（看成一个整形数字 🔢 就可以了）
    Green,
    Blue,
    Yellow
}；
int main() 
{
     enum color myColor1, myColor2; //定义一个枚举类型变量
}
```

可以直接定义枚举变量，不要枚举名，但不推荐！

```rust
enum
{
    Red,   
    Green,
    Blue,
    Yellow
}myColor1, myColor2；
```

这些都是常量，不能被改变。

![](vx_images/107184216242490.webp)

### 可以直接给枚举型变量赋值

`myColor1 = Red；`

### 可以手工给枚举赋值

```cpp
enum
{
    Red = -8,
    Green,
    Blue,
    Yellow
};
int main() 
{
     printf("%d\n", Red); //-8
     printf("%d\n", Green); //-7
     printf("%d\n", Blue); //-6
     printf("%d\n", Yellow); //-5
}
```

```cpp
enum
{
    Red = -8,
    Green,
    Blue = 12,
    Yellow
};
int main() 
{
     printf("%d\n", Red); //-8
     printf("%d\n", Green); //-7
     printf("%d\n", Blue); //12
     printf("%d\n", Yellow); //13
}
```

![](vx_images/101064216230400.webp)

但失去了枚举的意义，不建议这么做

### typedef 来定义类型

定义新的类型名，可以代替已经有的类型名。

```cpp
int main() 
{
    typedef int INTERGER;
    INTERGER a = 0;

    typedef struct date
    {
        int month;
        int year;
        int day;
    } DATE;
    // struct date birthday;  //之前这么定义
    DATE birthday;

    //变形，要记一记
    typedef int NUM[100]; //定义 NUM 为整形数组类型
    NUM n; // int n[100];

    typedef char* PSTRING; //定义PSTRING为字符指针类型
    PSTRING p;// char* p

    typedef int (*POINTER)(); //重新定义指向函数的指针类型
    POINTER q; // int (*q)() 指向返回值为整数的函数指针
}
```

先写出常规的定义方法，再替换自己想要的类型名，再加上`typedef`。  
a) 类型别名一般大写；  
b) `typedef` 是定义类型，不是变量；  
c) `typedef` 只是对已经存在的类型增加一个类型名，并没有创造新的类型；  
d) `typedef` 是编译时处理的；  
可执行文件：编译（预处理\[`#define`,`#include`,`#ifdef`\]，编译\[`typedef`\]，汇编)，链接  
e) `typedef` 主要作用：程序的通用性，可移植性。  
**比如：**如果一个程序发现 `int` 不够了，要变成 `double`，这下就要把所有的 `int` 都逐个改成 `double`，而 `typedef` 一只需改一个地方；

![](vx_images/86964216230261.webp)

\_\_int64 ---- 32位4字节，变成64位8字结

***

## 位运算

一个 int 数据 占 4个字节内存；  
一个 char 数据占1个字节内存；

![](vx_images/81654216234010.webp)

![](vx_images/76554216244712.webp)

字节还可以继续拆分。

> **定义:** 一个字节由 `8` 个二进制 位 组成。  
> 最左边称为最高位，最右边成为最低位。  
> 每一个二进制位的值是 `0` 或者 `1`。

一个`字节`能表示的最大的二进制数  
二进制：00000000 - 11111111  
十进制：0 - 255

`unsigned int abcd` 一个`整数int`能表示的最大二进制数是：  
二进制：111111111，11111111，11111111，11111111  
十进制： 42亿多。。

![](vx_images/65464216262499.webp)

### 位运算符介绍

针对【二进制位】来运算。

![](vx_images/15674116235544.webp)

![](vx_images/563554016234935.webp)

### 位运算符应用

算法，密码学应用很多。  
例子：完成十个任务，不用到数据库的情况下，把十个任务存到一个 `int`里面，`0` 代表未完成，`1` 代表完成。判断里面十个任务都为 `1` 时，操作完成。

## 文件概述 📃

**文件：**看成字符序列（字符流），对计算机 💻 讲所有文件都是字符流，都是二进制文件。

根据数据组织形式，我们（human👨👩）把文件分成两种：  
a) `ASCII文件`（文本文件）：，每一个字节，存放一个 ASCII 码，代表一个字符，这种文件一打开，人类就能看懂里面的内容  
b) `二进制文件`：把内存中的数据，按照其在内存中的存储形式原样输出到磁盘上存放。

人类可以选择时以`二进制形式`写入还是`文本形式`写入。  
同样，打开文件往外读取也可以选择以`文本形式`,还是以`二进制形式`。

![](vx_images/545434016242050.webp)

查看二进制文本内容;

![](vx_images/537334016233103.webp)

![](vx_images/529234016230599.webp)

![](vx_images/522114016240069.webp)

文本 10000 占了 5个字节； short int a = 10000；才占2字节

![](vx_images/512024016246935.webp)

如果通过`程序代码`，并加一些二进制`标记`读取文本文件的话，那么系统会认为你是以`二进制形式`来打开文件。

之所以能我们能读懂文本文件，是因为计算机做了转换，把十六进制 31 转成了我们读的懂的 1。这是要付出转化代价的。

每个字节一个地址：

![](vx_images/502924016253389.webp)

**小端存储 Little endian：**低的存低地址里，高的存高地址里。

![](vx_images/491834016258253.webp)

低字节0x10存在了低地址 0x0038F8D4 上

![](vx_images/478744016235813.webp)

高字节0x27存在了高地址 0x0038F8D5 上

**大端存储 Big endian：**高的存低地址，低的存高地址。  
**区别：**跟硬件有关。（网络数据通讯传输有关，一个电脑传输数据到另一台电脑上，存的这种方法必须统一，有专门函数）（了解）

#### 文件的打开/关闭

读写之前必须打开 🔛 。  
读写之后必须关闭 📴 。

**一，fopen：文件打开函数**

> 一旦用 `fopen` 函数，我们告诉系统三个信息：  
> a. 打开的文件名  
> b. 使用文件的方式（只读，写）  
> c. 让一个指针指向被打开的文件

```cpp
int main() 
{
    FILE* fp；// FILE 是个结构，fp是指向结构FILE的指针变量（结构体指针）
    fp = fopen("a.txt", "r"); //返回一个指针  文件名， 使用的文件的方式 都是字符串
                              // fp 就指向了这个文件
                              // 系统会自动开辟一块 sizeof(FILE) 大小的内存
                              //（记录📝指针位置，文件名，文件打开方式，文件当前位置，这些内容）
}
```

![](vx_images/467644016254572.webp)

文件打开方式们

**fgetc()**是每次一个字节的读。其实就是指针自动的移动了一个字节

**二，fclose：文件关闭函数**

只有 `fopen` 成功的文件才需要关闭。

> 关闭文件的原因：  
> ① `释放该文件占用的内存单元。`(要养成资源用完就释放的好习惯)  
> ② `防止往文件中写内容时文件内容写入补全。`(先写到缓存区，缓存要满了才写入一次，关闭文件的动作会触发系统把缓冲区的数据立即写入到磁盘上)

**三，文件的读写简单讲**  
**fputc():** 把一个字符写到磁盘文件上。  
**fgetc():** 从指定文件读一个字符。  
**feof():** 文件是否【读】结束。结束返回1(真)，没结束返回0(假)。

```cpp
int main() 
{
    FILE* fp;
    fp = fopen("a.txt", "w");//没指定路径就是当前目录
    if (fp == NULL){
        printf("文件打开失败");
    } else {
        char res = fputc('a', fp); //成功返回值就是 写入的字符的 ASCII，比如97
                                    //失败返回 EOF（end of file）
        if(res == EOF){
            //写失败
        }
        res = fputc('b', fp);
        res = fputc('c', fp);
        fclose(fp);// 返回也有值，只是不用关心，比如返回 1 关闭，0 失败
    }

    fp = fopen("a.txt", "r");//复用 fp
    if (fp == NULL){
        printf("文件打开失败");
    } else {
         char res = fgetc(fp);//每独处一个字符，返回这个字符，文件指针自动向下移动一个字符

        //  while (res != EOF){ 最好不要用 EOF 判读 读结束，因为 r 是只读，是打开
                                // 文本方式，二进制方式打开（b参数），万一以后用二进制方式打开，
                                // EOF是 -1，但却是被"翻译"成二进制里的一个字符，
                                // 这个此时 -1 就不代表文件结束。
        //  改造 （文件没结束）                       
         while (!feof(fp)){
             putchar(res);//往屏幕上输出一个字符
             res = fgetc(fp);//读下一个，因为指针已经在刚才移到下一个
         }
         printf("\n");
         fclose(fp);
    }
}
```

![](vx_images/458554016257070.webp)

![](vx_images/434374016259466.webp)

#### 商业代码（配置文件的读取）

![](vx_images/420224016241586.webp)

0D 干掉，保留0A

![](vx_images/389124016245832.webp)

换行 和 回车 就是 10 或者 13

![](vx_images/377994016249277.webp)

![](vx_images/347904016256608.webp)

```cpp
int main() 
{
    FILE* fp;
    fp = fopen("config.txt", "r");
    if (!p){
      printf("文件打开失败")  
    } else {
        char LineBuff[1024]；//保证容纳一行内容
        while (!feof(fp)){
            LineBuff[0] = 0; //'\0'
            //读取一行, 遇到换行符结束(0A也就是\0)；  还要存 '\0'
            if(fgets(LineBuff, sizeof(LineBuff)-1, fp) == NULL) 
                continue;
            if(LineBuff[0] == 0)
                continue;
lblprocstring:
            if(strlen(LineBuff) > 0){ //读到了实际内容
                //如果末尾是10换行符 或 13回车
                if(LineBuff[strlen(LineBuff)-1] == 10 || LineBuff[strlen(LineBuff)-1] == 13){
                    LineBuff[strlen(LineBuff)-1] = 0;//干掉 \ 反斜杠
                    goto lblprocstring;
                }
            }

            if(strlen(LineBuff) <= 0)//证明是一段空行
                continue;
            
            printf("%s\n", LineBuff);
        }//end while

        fclose(fp);
    }//end if
}
```

![](vx_images/334784016236442.webp)

***

## 将结构体写入二进制文件和读取

向文件中写入数据：

### fwrite(buffer,size,count,fp);

**butter :** 指针，要写到文件中去的数据就在这个地址里保存着。  
**size :** 要写入文件的字节数。  
**count ：** 要写入多少个 `size` 字节的数据项。  
**fb:** 文件指针。  
**返回值：**失败 0，成功 `count`。

```cpp
#pragma pack(1) // 1 字节对齐结构体
struct stu
{
    char name[30];
    int age;
    double score;
};
#pragma pack() // 这样结构体不会被增加字节

int main() 
{
    struct stu student[2];
    strcpy(student[0].name, "张三abc");
    student[0].age = 21;
    student[0].score = 92.1f;    

    strcpy(student[1].name, "李四def");
    student[1].age = 19;
    student[1].score = 85.2;    

    FILE* fp;
    fp = fopen("structfile.bin", "wb"); //写入二进制文件
    if(!fp){
        printf("Error");
    }else{   //两个返回 2                写入两个 students
        int result = fwrite(student, sizeof(struct stu), 2, fp);
        fclose(fp);
    }

    struct stu newStudent[2];
    fp = fopen("structfile.bin", "rb");
    if (fp == NULL) {
        printf("Error");
    }else{
       int result = fread(newStudent, sizeof(struct stu), 2, fp);
       fclose(fp);
    }
}
```

![](vx_images/327444016248575.webp)

**坑点：**

1.  结构体里面【写入文件的时候】，结构成员里不要有指针！  
    因为存的地址，是个随机的地址，写进去的话，下次不一定又是这个地址！无意义；
    
2.  结构体内存对齐问题，和编译器有关；  
    因为 sizeof 在Linux gcc，或者visual code里面的编译器不一样，对于 int double 或者 char 这些编译器会根据效率自己增加字节数，比如 int 从 4 字节变成 8 字节。  
    2.1 解决办法 ① ：强制无论在什么平台，都按 1 字节对齐（不对齐）。#pragma pack(1)  
    2.2 解决办法 ② ：不要跨平台。
    

向文件中读取数据：

### fread(buffer,size,count,fp);

**butter :** 指针，从文件中读出来的数据写到哪个地址。  
**size :** 要写入文件的字节数。  
**count ：** 要写入多少个 `size` 字节的数据项。  
**fb:** 文件指针。  
**返回值：**失败 0，成功 `count` 值。

***

END ✿✿ヽ(°▽°)ノ✿  
总`43`节 `27`小时

***

2020年10月20日23:17:27

***

#### 补充函数：

**memset()：**对一段内存地址空间全部设置为某个值或清空（否则会出现野值）

```cpp
int a[10] = {0};//static int a[10] 
memset(a, 0, sizeof(a[0])*10); //同上，也是把a数组全部用0填满
```

**time()：**返回当前的日历时间，错误时返回0

```cpp
#include<time.h>
```

**strerror():** 得到一个可读的错误提示信息，而不是一个错误的编号数字

```cpp
#include<string.h>
#include<errno.h> 中有一个全局变量 errno 记录错误编号 
```

**malloc():** 动态内存分配，成功返回指向被分配内存的指针(此存储区域初始值不确定)，否则返回空指针 `NULL`。当内存不再使用时，印使用 `free()` 函数释放。

```cpp
#include<malloc.h>
```

0人点赞

[💻 C语言](/nb/48132087)

  
  
作者：张小薇的coco  
链接：https://www.jianshu.com/p/066433ee9419  
来源：简书  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明�