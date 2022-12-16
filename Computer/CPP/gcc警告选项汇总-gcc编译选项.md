参考原文：https://blog.csdn.net/qq\_17308321/article/details/79979514

# 一、[GCC](https://blog.csdn.net/bandaoyu/article/details/115419255#GCC%E7%BC%96%E8%AF%91%E9%80%89%E9%A1%B9)警告选项

警告：不是错误的，但是有风险或表明可能有错误。

英文原文：[Warning Options - Using the GNU Compiler Collection (GCC)](http://gcc.gnu.org/onlinedocs/gcc-4.6.3/gcc/Warning-Options.html#Warning-Options "Warning Options - Using the GNU Compiler Collection (GCC)")

加上-Wall吧，gcc 默认不加参数的情况下 连定义了返回值的函数没有返回值都不报错。

（[gcc警告选项汇总\_靑い空゛-CSDN博客\_gcc 警告](https://blog.csdn.net/qq_17308321/article/details/79979514 "gcc警告选项汇总_靑い空゛-CSDN博客_gcc 警告")）

## **开启和关闭告警方法**

**1、-w （小写）禁止所有警告消息。**

**2、以“-W”(****大写）开头开启特定的警告；**

例如：

\-Wreturn-type(返回值告警),

\-Wsign-compare（有符号和无符号对比告警）

\-Wall (除extra外的所有告警)

\-Wextra (all外的其他告警)

> 如$ gcc -Wall test.c -o test

**3、以“-Wno-”开头关闭特定的警告;**

例如：

\-Wno-return-type （取消返回值告警）

\-Wno-sign-compare（取消有符号和无符号对比告警）

> 如：$ gcc -Wall -Wno-unused test.c -o test

## 批量**开启告警（即**\-Wall和-Wextra 批量开启的告警**）**

某些选项（如-Wall和-Wextra ）会打开其他选项，例如-Wunused ，这可能会启用其他选项，例如-Wunused-value 。

**\-Wall** 

**（**[gcc -Wall详解\_jiedu\_新浪博客](http://blog.sina.com.cn/s/blog_553230d70101efqv.html "gcc -Wall详解_jiedu_新浪博客")**）**

该选项相当于同时使用了下列所有的选项：

> ◆unused-function：遇到仅声明过但尚未定义的静态函数时发出警告。  
> ◆unused-label：遇到声明过但不使用的标号的警告。  
> ◆unused-parameter：从未用过的函数参数的警告。  
> ◆unused-variable：在本地声明但从未用过的变量的警告。  
> ◆unused-value：仅计算但从未用过的值得警告。  
> ◆Format：检查对printf和scanf等函数的调用，确认各个参数类型和格式串中的一致。  
> ◆implicit-int：警告没有规定类型的声明。  
> ◆implicit-function-：在函数在未经声明就使用时给予警告。  
> ◆char-subscripts：警告把char类型作为数组下标。这是常见错误，程序员经常忘记在某些机器上char有符号。  
> ◆missing-braces：聚合初始化两边缺少大括号。  
> ◆Parentheses：在某些情况下如果忽略了括号，编译器就发出警告。  
> ◆return-type：如果函数定义了返回类型，而默认类型是int型，编译器就发出警告。同时警告那些不带返回值的 return语句，如果他们所属的函数并非void类型。  
> ◆sequence-point：出现可疑的代码元素时，发出报警。  
> ◆Switch：如果某条switch语句的参数属于枚举类型，但是没有对应的case语句使用枚举元素，编译器就发出警告（在switch语句中使用default分支能够防止这个警告）。超出枚举范围的case语句同样会导致这个警告。  
> ◆strict-aliasing：对变量别名进行最严格的检查。  
> ◆unknown-pragmas：使用了不允许的#pragma。  
> ◆Uninitialized：在初始化之前就使用自动变量。

**（**不要被它的表面意思迷惑，下面是使用-Wall选项的时候没有生效的一些警告项,而且还有`-Wextra`**）**

> ◆cast-align：一旦某个指针类型强制转换时，会导致目标所需的地址对齐边界扩展，编译器就发出警告。例如，某些机器上只能在2或4字节边界上访问整数，如果在这种机型上把char \*强制转换成int \*类型， 编译器就发出警告。  
> ◆sign-compare：将有符号类型和无符号类型数据进行比较时发出警告。  
> ◆missing-prototypes ：如果没有预先声明函数原形就定义了全局函数，编译器就发出警告。即使函数定义自身提供了函数原形也会产生这个警告。这样做的目的是检查没有在头文件中声明的全局函数。  
> ◆Packed：当结构体带有packed属性但实际并没有出现紧缩式给出警告。  
> ◆Padded：如果结构体通过充填进行对齐则给出警告。  
> ◆unreachable-code：如果发现从未执行的代码时给出警告。  
> ◆Inline：如果某函数不能内嵌（inline），无论是声明为inline或者是指定了-finline-functions 选项，编译器都将发出警告。  
> ◆disabled-optimization：当需要太长时间或过多资源而导致不能完成某项优化时给出警告。
> 
> 上面是使用-Wall选项时没有生效，但又比较常用的一些警告选项。

**\-Wextra**

但不要被-Wall的名字迷惑，它并没有开启所有告警，`-Wextra用于`启用一些未由-Wall启用的额外警告标志。 （此选项过去称为-W ，旧名称仍然受支持，但更新的名称更具描述性。）

> \-Wclobbered    
> \-Wcast-function-type    
> \-Wempty-body    
> \-Wignored-qualifiers  如果函数的返回类型具有类型限定符（如const ，则发出警告。 对于ISO C这样的类型限定符没有效果，因为函数返回的值不是左值。 对于C ++来说，警告只是针对标量类型或void发出的。 ISO C禁止在函数定义上使用合格的void返回类型，所以这种返回类型总是会在没有这个选项的情况下收到警告。  
> \-Wimplicit-fallthrough=3   
> \-Wmissing-field-initializers    
> \-Wmissing-parameter-type (C only)    
> \-Wold-style-declaration (C only)    
> \-Woverride-init    
> \-Wsign-compare (C only)   
> \-Wtype-limits   由于数据类型范围有限而导致比较始终为真或始终为false，但不警告常量表达式。例如，警告如果将一个无符号变量与<或与0进行比较>=  
> \-Wuninitialized    
> \-Wshift-negative-value (in C++03 and in C99 and newer)    
> \-Wunused-parameter (only with -Wunused or -Wall)   
> \-Wunused-but-set-parameter (only with -Wunused or -Wall)  

选项-Wextra还会打印以下情况的警告消息：

  指针与整数零与< ， <= ， >或>= 。  
（仅限C ++）枚举器和非枚举器都出现在条件表达式中。  
（仅限C ++）不明确的虚拟基础。  
（仅限C ++）为已声明为register的数组下标。  
（仅限C ++）取得已声明register的变量的地址。  
（仅限C ++）基类不在派生类的复制构造函数中初始化。  
 

## 将告警转为错误

**\-Werror ：所有告警当错误报**

> 将所有的警告当成错误进行处理。一旦加上有警告就编译不通过。除非特别严格，一般不会使用。

**\-Werror=  将指定的警告转换为错误。**

> Format：检查对printf和scanf等函数的调用，确认各个参数类型和格式串中的一致。
> 
> printf("%d %d", 1);
> 
> error: format '%d' expects a matching 'int' argument \[-Werror=format\]
> 
> [c++ - Gcc忽略-Wno-unused-variable - IT工具网](https://www.coder.work/article/3353423 "c++ - Gcc忽略-Wno-unused-variable - IT工具网")

**反过来：**

\-Wno-error取消编译选项-Werror

实例2：

> 假设我们使用了一个人的代码A目录，里面有一个-Werror的选项，把所有的警告当做错误；又使用了另一个人的代码B目录，里面存在一堆Warning。这样，当我们把它们合在一起编译的时候，A中的-Werror选项会导致B的代码编译不过。但我们又不想去修改B的代码，怎么办？  
> *方法是，先add\_subdirectory(A)，之后，加上一句  
> set(CMAK\_CXX\_FLAGS "${CMAK\_CXX\_FLAGS} -Wno-error")  
> \-Wno-这个前缀，就是用来取消一个编译选项的  
> 然后，再add\_subdirectory(B)*

## 其他告警项

**\-Wfatal-errors 发生第一个错误时中止编译**

在发生第一个错误时中止编译。

**\-Wchkp**

警告由指针界限检查器（ -fcheck-pointer-bounds ）发现的无效内存访问。

**\-Wno-coverage-mismatch**

  
如果使用-fprofile-use选项时反馈配置文件不匹配，则警告 。 如果在使用-fprofile-gen编译和使用-fprofile-use编译时源文件发生更改，则具有配置文件反馈的文件可能无法与源文件匹配，并且GCC无法使用配置文件反馈信息。 默认情况下，此警告已启用并被视为错误。 -Wno-coverage-mismatch可用于禁用警告或-Wno-error = coverage-mismatch可用于禁用该错误。 禁用此警告的错误可能会导致代码质量不佳，并且仅在非常小的更改情况下才有用，例如修复现有代码库的错误。 不建议完全禁用该警告。

**\-Wno-cpp 禁止#warning指令发出的警告消息。**

（仅限于Objective-C，C ++，Objective-C ++和Fortran）

禁止#warning指令发出的警告消息。

\-Wshadow  
当一个局部变量遮盖住了另一个局部变量，或者全局变量时，给出警告。很有用的选项，建议打开。 -Wall 并不会打开此项。

\-Wpointer-arith  
对函数指针或者void \*类型的指针进行算术操作时给出警告。也很有用。 -Wall 并不会打开此项。

\-Wcast-qual  
当强制转化丢掉了类型修饰符时给出警告。 -Wall 并不会打开此项。

\-Waggregate-return  
如果定义或调用了返回结构体或联合体的函数，编译器就发出警告。

\-Winline  
无论是声明为 inline 或者是指定了-finline-functions 选项，如果某函数不能内联，编译器都将发出警告。如果你的代码含有很多 inline 函数的话，这是很有用的选项。

\-Wundef  
当一个没有定义的符号出现在 #if 中时，给出警告。

\-Wredundant-decls  
如果在同一个可见域内某定义多次声明，编译器就发出警告，即使这些重复声明有效并且毫无差别。

\-Wstrict-prototypes  
如果函数的声明或定义没有指出参数类型，编译器就发出警告。很有用的警告。

\-Wctor-dtor-privacy （C++ only）  
当一个类没有用时给出警告。因为构造函数和析构函数会被当作私有的。

\-Wnon-virtual-dtor（C++ only）  
当一个类有多态性，而又没有虚析构函数时，发出警告。-Wall会开启这个选项。

\-Wreorder（C++ only）  
如果代码中的成员变量的初始化顺序和它们实际执行时初始化顺序不一致，给出警告。

\-Wno-deprecated（C++ only）  
使用过时的特性时不要给出警告。

\-Woverloaded-virtual（C++ only）  
如果函数的声明隐藏住了基类的虚函数，就给出警告。

\-Winit-self (C, C++, Objective-C and Objective-C++ only)  
警告使用自己初始化的未初始化变量。 请注意，此选项只能与-Wuninitialized选项一起使用。

例如，只有在指定-Winit-self时，GCC才会在下面的代码片段中提醒i未初始化：

  int f（）  
 {  
   int i = i;  
  回报我;  
 }  
  
此警告由C ++中的-Wall启用。

……

更多手册翻译：https://blog.csdn.net/qq\_17308321/article/details/79979514

## **作用顺序和覆盖**

具体的选项优先于不特定的选项，与命令行中的位置无关。

对于相同特征的选项，最后一个生效。

# 二、GCC编译选项

[GCC 告警调试优化选项详细说明\_me败家懒妞的博客-CSDN博客](https://blog.csdn.net/weixin_42615308/article/details/83151569 "GCC 告警调试优化选项详细说明_me败家懒妞的博客-CSDN博客")

**\-pedantic**

> 以ANSI/ISO C标准列出的所有警告
> 
> 当GCC在编译不符合ANSI/ISO C语言标准的源代码时，如果在编译指令中加上了-pedantic选项，那么源程序中使用了扩展语法的地方将产生相应的警告信息。
> 
> [gcc -wall -pedantic -ansi\_R-G-Y-CQ\_新浪博客](http://blog.sina.com.cn/s/blog_9151e7300101kns2.html "gcc -wall -pedantic -ansi_R-G-Y-CQ_新浪博客")
> 
> 如果代码中的成员变量的初始化顺序和它们实际执行时初始化顺序不一致，给出警告。

## **GCC常用选项**

> \--help  
> \--target-help  
> 显示 gcc 帮助说明。‘target-help’是显示目标机器特定的命令行选项。
> 
> \--version  
> 显示 gcc 版本号和版权信息 。
> 
> \-o outfile  
> 输出到指定的文件。
> 
> \-x language  
> 指明使用的编程语言。允许的语言包括：c c++ assembler none 。 ‘none’意味着恢复默认行为，即根据文件的扩展名猜测源文件的语言。
> 
> \-v  
> 打印较多信息，显示编译器调用的程序。
> 
> \-###  
> 与 -v 类似，但选项被引号括住，并且不执行命令。
> 
> \-E  
> 仅作预处理，不进行编译、汇编和链接。如上图所示。
> 
> \-S  
> 仅编译到汇编语言，不进行汇编和链接。如上图所示。
> 
> \-c  
> 编译、汇编到目标代码，不进行链接。如上图所示。
> 
> \-pipe  
> 使用管道代替临时文件。
> 
> \-combine  
> 将多个源文件一次性传递给汇编器。

其他GCC选项更多有用的GCC选项：

\-l library  
\-llibrary  
进行链接时搜索名为library的库。  
例子： $ gcc test.c -lm -o test

\-Idir  
把dir 加入到搜索头文件的路径列表中。  
例子： $ gcc test.c -I../inc -o test

\-Ldir  
把dir 加入到搜索库文件的路径列表中。  
例子： $ gcc -I/home/foo -L/home/foo -ltest test.c -o test

\-Dname  
预定义一个名为name 的宏，值为1。  
例子： $ gcc -DTEST\_CONFIG test.c -o test

\-Dname =definition  
预定义名为name ，值为definition 的宏。

\-ggdb  
\-ggdblevel  
为调试器 gdb 生成调试信息。level 可以为1，2，3，默认值为2。

\-g  
\-glevel  
生成操作系统本地格式的调试信息。-g 和 -ggdb 并不太相同， -g 会生成 gdb 之外的信息。level 取值同上。

\-s  
去除可执行文件中的符号表和重定位信息。用于减小可执行文件的大小。

\-M  
告诉预处理器输出一个适合make的规则，用于描述各目标文件的依赖关系。对于每个 源文件，预处理器输出 一个make规则，该规则的目标项(target)是源文件对应的目标文件名，依赖项(dependency)是源文件中 \`#include引用的所有文件。生成的规则可 以是单行，但如果太长，就用\`\\'-换行符续成多行。规则 显示在标准输出，不产生预处理过的C程序。

\-C  
告诉预处理器不要丢弃注释。配合\`-E'选项使用。

\-P  
告诉预处理器不要产生\`#line'命令。配合\`-E'选项使用。

\-static  
在支持动态链接的系统上，阻止连接共享库。该选项在其它系统上 无效。

\-nostdlib  
不连接系统标准启动文件和标准库文件，只把指定的文件传递给连接器。

## 优化项|优化等级

\-O0禁止编译器进行优化。默认为此项。  
\-O1尝试优化编译时间和可执行文件大小。

\-O2更多的优化，会尝试几乎全部的优化功能，但不会进行“空间换时间”的优化方法。

\-O3在 -O2 的基础上再打开一些优化选项：-finline-functions， -funswitch-loops 和 -fgcse-after-reload 。

\-Os对生成文件大小进行优化。它会打开 -O2 开的除了会那些增加文件大小的全部选项。

## 其他项

\-finline-functions  
把所有简单的函数内联进调用者。编译器会探索式地决定哪些函数足够简单，值得做这种内联。

\-fstrict-aliasing  
施加最强的别名规则（aliasing rules）。

## 标准Standard

\-ansi  
支持符合ANSI标准的C程序。这样就会关闭GNU C中某些不兼容ANSI C的特性。

\-std=c89 指明使用标准 ISO C90 作为标准来编译程序。

\-std=c99指明使用标准 ISO C99 作为标准来编译程序。

\-std=c++98指明使用标准 C++98 作为标准来编译程序。

\-std=gnu9x使用 ISO C99 再加上 GNU 的一些扩展。

\-fno-asm  
不把asm, inline或typeof当作关键字，因此这些词可以用做标识符。用 \_\_asm\_\_， \_\_inline\_\_和\_\_typeof\_\_能够替代它们。 \`-ansi' 隐含声明了\`-fno-asm'。

\-fgnu89-inline  
告诉编译器在 C99 模式下看到 inline 函数时使用传统的 GNU 句法。

## C options

\-fsigned-char

把char定义为有符号类型，如同signed char  
\-funsigned-char  
把char定义为无符号类型，如同unsigned char。

\-traditional  
尝试支持传统C编译器的某些方面。详见GNU C手册。

\-fno-builtin  
\-fno-builtin-function  
不接受没有 \_\_builtin\_ 前缀的函数作为内建函数。

\-trigraphs  
支持ANSI C的三联符（ trigraphs）。\`-ansi'选项隐含声明了此选项。

\-fsigned-bitfields  
\-funsigned-bitfields  
如果没有明确声明\`signed'或\`unsigned'修饰符，这些选项用来定义有符号位域或无符号位域。缺省情况下，位域是有符号的，因为它们继承的基本整数类型，如int，是有符号数。

\-fno-strict-aliasing 

“-fstrict-aliasing”表示启用严格别名规则，“-fno-strict-aliasing”表示禁用严格别名规则，当gcc的编译优化参数为“-O2”、“-O3”和“-Os”时，默认会打开“-fstrict-aliasing”。

防止出现此类错误：[GCC编译选项--"-fno-strict-aliasing"\_leafmaple的专栏-CSDN博客\_strict-aliasing](https://blog.csdn.net/liaofengjun/article/details/52743452 "GCC编译选项--"-fno-strict-aliasing"_leafmaple的专栏-CSDN博客_strict-aliasing")

## C++ options

\-fsyntax-only  
检查代码中的语法错误，但除此之外不要做任何事情。

\-ffor-scope  
从头开始执行程序，也允许进行重定向。

\-fno-rtti  
关闭对 dynamic\_cast 和 typeid 的支持。如果你不需要这些功能，关闭它会节省一些空间。

Machine Dependent Options (Intel)

\-mtune=cpu-type  
为指定类型的 CPU 生成代码。cpu-type 可以是：i386，i486，i586，pentium，i686，pentium4 等等。

\-msse  
\-msse2  
\-mmmx  
\-mno-sse  
\-mno-sse2  
\-mno-mmx  
使用或者不使用MMX，SSE，SSE2指令。

\-m32  
\-m64  
生成32位/64位机器上的代码。

\-mpush-args  
\-mno-push-args  
（不）使用 push 指令来进行存储参数。默认是使用。

\-mregparm=num  
当传递整数参数时，控制所使用寄存器的个数。  
原文链接：https://blog.csdn.net/qustdjx/article/details/8058122

**\-fPIC**

> \-fPIC 作用于编译阶段，告诉编译器产生与位置无关代码(Position-Independent Code)，则产生的代码中，**没有绝对地址，全部使用相对地址，故而代码可以被加载器加载到内存的任意位置，都可以正确的执行。这正是共享库所要求的，共享库被加载时，在内存的位置不是固定的。**
> 
> ```
> gcc -shared -fPIC -o 1.so 1.c
> ```
> 
> 这里有一个-fPIC参数  
> PIC就是position independent code  
> PIC使.so文件的代码段变为真正意义上的共享  
> **如果不加-fPIC,则加载.so文件的代码段时,代码段引用的数据对象需要重定位,** **重定位会修改代码段的内容,这就造成每个使用这个.so文件代码段的进程在内核里都会生成这个.so文件代码段的copy**.每个copy都不一样,取决于 这个.so文件代码段和数据段内存映射的位置.  
> 也就是  
> 不加fPIC编译出来的so,是要**再加载时根据加载到的位置再次重定位**的.(因为它里面的代码并不是位置无关代码)  
> 如果被多个应用程序共同使用,那么它们必须每个程序维护一份.so的代码副本了.(因为.so被每个程序加载的位置都不同,显然这些重定位后的代码也不同,当然不能共享)  
> 我们总是用fPIC来生成so,也从来不用fPIC来生成.a;fPIC与动态链接可以说基本没有关系,libc.so一样可以不用fPIC编译,只是这样的so必须要在加载到用户程序的地址空间时重定向所有表目.
> 
> 因此,不用fPIC编译so并不总是不好.
> 
> > 如果你满足以下4个需求/条件:  
> > 1.该库可能需要经常更新  
> > 2.该库需要非常高的效率(尤其是有很多全局量的使用时)  
> > 3.该库并不很大.  
> > 4.该库基本不需要被多个应用程序共享
> > 
> > 如果用没有加这个参数的编译后的共享库，也可以使用的话，可能是两个原因：  
> > 1：gcc默认开启-fPIC选项  
> > 2：loader使你的代码位置无关
> 
> [gcc编译参数-fPIC的一些问题\_哒哒的博客-CSDN博客\_fpic](https://blog.csdn.net/derkampf/article/details/69660050 "gcc编译参数-fPIC的一些问题_哒哒的博客-CSDN博客_fpic")