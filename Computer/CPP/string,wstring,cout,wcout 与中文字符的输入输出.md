转自：string,wstring,cout,wcout 与中文字符的输入输出

## 首先说明是什么string与wstring

在C++标准里定义了两个字符串string和wstring 
~~~
typedef basic_string<char> string; 

typedef basic_string<wchar_t> wstring; 
~~~
前者string是常用类型，可以看作char\[\]，其实这正是与string定义中的\_Elem=char相一致。而wstring，使用的是wchar\_t类型，这是宽字符，用于满足非ASCII字符的要求，例如Unicode编码，中文，日文，韩文什么的。对于wchar\_t类型，实际上C++中都用与char函数相对应的wchar\_t的函数，因为他们都是从同一个模板类似于上面的方式定义的。因此也有wcout, wcin, werr等函数。      

实际上string也可以使用中文，但是它将一个汉字写在2个char中。而如果将一个汉字看作一个单位wchar\_t的话，那么在wstring中就只占用一个单元，其它的非英文文字和编码也是如此。

## 什么是locale

C/C++程序中，locale将决定程序所使用的当前语言编码、日期格式、数字格式及其它与区域有关的设置，locale设置的正确与否将影响到程序中字符串处理（wchar\_t如何输出、strftime()的格式等）。因此，对于每一个程序，都应该慎重处理locale设置。C locale和C++ locale是独立的。C locale用setlocale(LC\_CTYPE, “”)初始化，C++ locale用std::locale::global(std::locale(“”))初始化。这样就可以根据当前运行环境正确设置locale。

## 什么是imbue

**imbue**　扩展词汇　
- 英 [ɪm'bjuː] 　 　 美 [ɪm'bjuː] 　 　
- v. 灌输；使感染；使充满

imbue函数是指对象引用,表示输出时,使用的区域语言对象。  
函数原型:  
`locale basic_ios::imbue(const locale&loc); `
参数说明:  
loc: const 对象引用,表示输出时,使用的区域语言对象  
返回值:之前的使用的区域语言

c++中，可以直接利用string及cout进行中文的存储及输出：

```
#include <iostream>  
#include <string>  
using namespace std;  
  
void main()  
{  
    string s1="第一";  
    cout<<s1<<endl;   
}  
```

正常输出：  
第一  
但是有些时候不得不用到wstring来存储中文字符，这时输出需要  
导入locale头文件  
中文字符前需要加L，并用wstring存储  
输出前更改本地语言，wcout.imbue(locale("chs"))  
用wcout输出

```
#include <iostream>   
#include <string>   
#include <locale>   
using namespace std;  
  
void main()  
{  
    string s1="第一";  
    wstring s2=L"第二";  
    cout<<s1<<endl;  
    wcout.imbue(locale("chs"));  
    wcout<<s2<<endl;  
}  
```

结果便是：

> 第一
> 
> 第二

wstring 返回“第二”的size为2.如果是string，返回的size为4.