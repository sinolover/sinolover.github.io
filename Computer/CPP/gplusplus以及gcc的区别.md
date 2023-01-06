g++以及gcc的区别
转自：[g++以及gcc的区别 - 知乎](https://zhuanlan.zhihu.com/p/100050970 "g++以及gcc的区别 - 知乎")

GCC ，gcc 和g++：

一直没搞清这几个东西的概念，搜了半天看到了一个不错的解释，所以大致记录一下，以免以后再忘记，[链接](https://link.zhihu.com/?target=https%3A//www.cnblogs.com/samewang/p/4774180.html "链接")。（原谅没找到原文出处）

**GCC**:GNU Compiler Collection(GUN 编译器集合)，它可以编译C、C++、JAVA、Fortran、Pascal、Object-C等语言。

**gcc**是GCC中的GUN C Compiler（C 编译器）

**g++**是GCC中的GUN C++ Compiler（C++编译器）

由于编译器是可以更换的，所以gcc不仅仅可以编译C文件

所以，更准确的说法是：gcc调用了C compiler，而g++调用了C++ compiler

gcc和g++的主要区别

1\. 对于 \*.c和\*.cpp文件，gcc分别当做c和cpp文件编译（c和cpp的语法强度是不一样的）

2\. 对于 \*.c和\*.cpp文件，g++则统一当做cpp文件编译

3\. 使用g++编译文件时，**g++会自动链接标准库STL，而gcc不会自动链接STL**

4\. gcc在编译C文件时，可使用的预定义宏是比较少的

5\. gcc在编译cpp文件时/g++在编译c文件和cpp文件时（这时候gcc和g++调用的都是cpp文件的编译器），会加入一些额外的宏。

6.在用gcc编译c++文件时，为了能够使用STL，需要加参数 –lstdc++ ，但这并不代表 gcc –lstdc++ 和 g++等价，它们的区别不仅仅是这个。

编译工具套件提供的各个命令作用：  
  
\- Compiler 编译程序，gcc/g++/cc 用来编译源代码文件，通常通过 gcc 调用 g++ 或 cc 命令；  
\- Assemblers 汇编程序，编译汇编程序，通常通过 gcc 调用 as 命令；  
\- Linkers 链接程序，用来链接编译输出的目标文件，生成可执行程序，通常通过 gcc 调用 ld 命令，还有 ar 命令生成链接库；  
  
GCC 编译套件不仅支持 C/C++，支持各种 C/C++ 方言标准，还支持 Go 或 Object-C/C++ 等，并且支持 x86、x86\_64、ARM 等多种 CPU 架构。提供 \`gcc\` 命令相当于一个门户，它本身就是 C 语言编译器，并且通过它可以调用整个编译流程中会使用到的各种命令。它可以识别各种 C/C++ 源文件的扩展名，并将相应参数传给相应的命令，如果是 C++ 源代码，则执行 \`g++\` 命令。  
  
另外，\`cc\` 是 Unix 系统的 C Compiler，一个是古老的 C 编译器命令。Linux 的 cc 一般是一个符号连接，指向 gcc，可以通过 \`ls -l /usr/bin/cc\` 来查看。  
  
注意，直接使用 \`g++\` 编译 C 语言源代码会被当作 C++ 源代码处理。  
  
例如：  
1.只进行预编译，不生成汇编程序、目标文件和可执行程序，只需要执行命令时使用 \`-E\`：  
2.编译 C 语言为汇编程序，不生成目标文件和可执行程序，只需要执行命令时使用 \`-S\`：