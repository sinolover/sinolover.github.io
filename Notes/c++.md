# c++
化这个变量,这时需要 decltype 变量,它的作用是选择并返回操作数的数据类型。在此过程中编译器分析表达式得到其类型,但没有实际计算表达式的值。使用 decltype 时需要紧跟个圆括号,括号内为一个表达式,声明的变量与该表达式类型一致。一个 decltype 变量声明的例子如下:
decltype(i) j=2;
上述声明表示j以 2 作为初始值,类型与i一致

constexpr 函数是指能用于常量表达式的函数。定义 constexpr 函数的方法与其他函数类似,但要遵循几项约定:函数的返回类型以及所有的形参类型必须是常量,而且函数体中必须有且仅有一条 return 语句 :
constexpr int get size() freturn 20;1
constexpr int foo=get size();
//正确: foo 是一个常量表达式

![](vx_images/244294709257714.png)