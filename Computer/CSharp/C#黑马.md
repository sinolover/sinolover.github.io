w3svc world wide web publishing Service

ISAPI InternetServer ApplicationProgramming Interface

WAS Windows Process Activation Service

PipleRuntime -》 HttpRuntime -》 HttpApplication

HttpContext HttpModule

MVC模式中元素之间“混乱”的交互主要体现在允许View 和 Model 绕开Controller 进行单独的“交流“，这在MVP中得到了彻底解决。Model仅与Presenter交互，View只能通过Presenter间接的调用Model。Model不仅仅与可视化元素的呈现（View）无关，与UI处理逻辑（Presenter）也无关。使用MVP的应用是用户驱动的，而非Model驱动的，所以Model不需要主动通知View已提醒状态发生了改变。按照View和Presenter之间的交互方式以及View本身的职责范围，Martin Folower将MVP分为PV（Passive View）和（SupervisingController）两种模式

2.1面向对象

子类 is a 父类

实现 can do 接口

类 has a 成员变量/成员方法

C#中的静态变量/静态方法，只能通过类名进行调用（在看别人的代码是非常有用，当一个 类.成员函数 的时候，说明，这是个静态的方法

同时const关键字与static关键字具有相同的功能，因此定义为const的变量，也必须通过类名进行引用

readonly 关键字，可以将字段声明为只读 public readonly int \_age;

sealed 关键字 等同于final

C#不支持 默认参数

params 1.必须跟一个数组 2.必须是最后一个参数 3.最多只能有一个params定义

ref out：在定义及使用时，均需带有ref out 关键字 使用： GetName(ref/out myname); 定义： public string GetName(ref/out \_name);

C#类的初始化过程

1.  初始化全部静态字段
2.  调用静态构造函数
3.  初始化全部实例字段
4.  调用实例构造函数

静态构造函数的几点说明：

*   静态构造函数会在类的第一个实例被创建之前被调用
*   静态构造函数只能调用一次
*   静态类成员初始化之后才会调用静态构造函数
*   静态构造函数将在引用任何的静态类成员之前被调用

编译器会将不同文件中，同一个命名空间内的所有代码累加到一起

所有 使用new 关键字创建的变量都是引用类型 string 完整创建方法是 string str =new string(""); 简写为 string str = "";所以string也是引用类型

C#可以快速的返回内存给New是因为托管对类似于简单的字节数组，有一个指向第一个可用内存的指针

内存分配过程（C#是基于分代的垃圾回收，分0 1 2 三代）

1.  当0代中没有可以分配的有效内存时，在0代中触发一次垃圾回收
2.  当0代对象移动到1代时，会在1代中申请内存，若无法申请到足够内存，则会在1代中触发一次垃圾回收
3.  当1代对象移动到2代时，会在2代中申请内存，若无法申请到足够内存，则会触发一个OutOfMemoryException异常

1.Ado.Net

Connection: SqlConnection（Using System.Data.SqlClient）

*   connectionstring : data source : 服务器 initial catalog : 数据库 Integrited Security = true 集成验证
*   open
*   close

Command

*   ExecuteNoneQuery:返回int 受影响的行数 set nocount on
*   ExecuteDataReader:返回DataReader
*   ExecuteScaled : 返回 select count(\*)

DataReader:

*   Read (类似Java hasNext)

Console：

*   readline writeline
*   readkey

快捷键 两次tab键

*   ctrl k c 注释 ctrl k u 取消注释
*   ctrl . 为未定义方法名，创建方法体 <=>shift alt f10（自动导入命名空间）
*   #region #endregion 定义可折叠区

重用代码段

*   crl console.readline cwr console.writeline crk console.readkey
*   wh while

默认为值传递，地址传递应使用ref

可变参数:关键字 params 例：static int GetMaxNum(params int\[\] pms);

数组的定义

*   完整定义int\[\] arr2 = new int\[2\]{1,2}
*   个数重复int\[\] arr2 = new int\[\]{1,2}
*   形式重复int\[\] arr2 = {1,2}
*   int\[\] arr1 = new int\[2\]
*   int\[\] arr4 = new Array()

String:字符串具有不可变性，类似于Java的特性 final

*   IndexOf LastIndexOf
*   CharAt , \[\]下标
*   trim 去掉两端空格
*   replace()s
*   split() 用一个字符分割 join() : split 的反方向，用一个字符连接
*   ToLower() ToUpper()
*   ToCharArray()

对 dictionary 遍历 使用 KeyValuePair 类型 foreach(KeyValuePir itm in dictionary)

new char\[\] {'1','2'} => 声明并初始化一个匿名数组