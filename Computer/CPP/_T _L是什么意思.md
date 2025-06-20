# \_T \_L是什么意思

C++语言中“\_T”是什么意思？

\_T("Hello")是一个宏,他的作用是让你的程序支持Unicode编码，因为Windows使用两种字符集ANSI和UNICODE，前者就是通常使用的单字节方式，但这种方式处理象中文这样的双字节字符不方便，容易出现半个汉字的情况。而后者是双字节方式，方便处理双字节字符。

Windows NT的所有与字符有关的函数都提供两种方式的版本，而Windows 9x只支持ANSI方式。

如果你编译一个程序为ANSI方式，_T实际不起任何作用。而如果编译一个程序为UNICODE方式，则编译器会把"Hello"字符串以UNICODE方式保存。

\_T和\_L的区别在于，_L不管你是以什么方式编译，一律以UNICODE方式保存。



1、C++语言中“_T”是什么意思？

Visual C++里边定义字符串的时候，用\_T来保证兼容性，VC支持ascii和unicode两种字符类型，用\_T可以保证从ascii编码类型转换到unicode编码类型的时候，程序不需要修改。

如果将来你不打算升级到unicode，那么也不需要_T，

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-

_t("hello world")

在ansi的环境下，它是ansi的，如果在unicode下，那么它将自动解释为双字节字符串，既unicode编码。

这样做的好处，不管是ansi环境，还是unicode环境，都适用。

2、请问在vc++中的字符串_T("ABC")和一个普通的字符串“ABC”有什么区别。

_T("ABC")

表示如果定义了宏 _UNICODE

它表示 L"ABC"，每个字符为16位，属于宽字符字符串

if not _UNICODE

它就是ascii的"ABC"，每个字符为8位

"ABC"就是指ascii字符串"ABC"


相当于
~~~ cpp
#ifdef _UNICODE
  #define _T("ABC") L"ABC"
#else
  #define _T("ABC") "ABC"
#endif
~~~




_T在tchar.h头文件中定义了：

#define __T(x)  L##x
#define \_T(x) \_\_T(x)

#define Conn(x,y) x##y
> x##y：表示x连接y
> 举例说：
> int n = Conn(123,456); 结果就是n=123456;
> char* str = Conn("asdf", "adf")结果就是 str = "asdfadf";

#define ToChar(x) #@x
> #@x：给x加上单引号，结果返回是一个const char。举例说：
> char a = ToChar(1);结果就是a='1';
> 做个越界试验char a = ToChar(123);结果是a='3';
> 但是如果你的参数超过四个字符，编译器就给给你报错了！error C2015: too many characters in constant ：P

#define ToString(x) #x
> #x：给x加双引号
> char* str = ToString(123132);就成了str="123132";
