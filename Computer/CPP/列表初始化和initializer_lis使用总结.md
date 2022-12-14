## 1. 变量初始化
   先看下C98里都是如何进行变量初始化的。

+ 数组初始化（使用了初始化列表）
~~~cpp
int i_arr[3] = { 1, 2, 3 };  //普通数组
  POD类型初始化（使用了初始化列表）【POD 类型即 plain old data 类型，简单来说，是可以直接使用 memcpy 复制的对象】
struct A
{
    int x;
    struct B
    {
        int i;
        int j;
    } b;
} a = { 1, { 2, 3 } };  //POD类型
~~~
+  内置类型初始化 
~~~cpp
int i=0;
   自定义对象初始化（使用拷贝构造函数） 
class Foo
{
    public:
    Foo(int) {}
} foo = 123;  //需要拷贝构造函数
~~~
+  直接初始化 
~~~cpp
int j(0);
Foo bar(123);
~~~
从上述罗列的初始化的方法中看，在C++98/03中我们只能对普通数组和POD(plain old data，简单来说就是可以用memcpy复制的对象)类型可以使用列表初始化。

所有的这些初始化方法，都有各自的适用范围和作用，而且这些种类繁多的初始化方法，没有一种可以通用所有情况。

那么为了统一初始化方式，并且让初始化行为具有确定的效果，C++11 中提出了列表初始化（List-initialization）的概念。 

 

## 2. 列表初始化
C++11 中，初始化列表的适用性被大大增加了。它可以用于任何类型对象的初始化。

**示例1：**
~~~cpp
// 自定义类Foo
class Foo
{
public:
    Foo(int) {}
private:
    Foo(const Foo &);
};

int main(void)
{
{
	Foo a1(123); //调用Foo(int)构造函数初始化
	Foo a2 = 123; //error Foo的拷贝构造函数声明为私有的，该处的初始化方式是隐式调用Foo(int)构造函数生成一个临时的匿名对象，再调用拷贝构造函数完成初始化
 
	Foo a3 = { 123 }; //列表初始化
	Foo a4 { 123 }; //列表初始化
 
	int a5 = { 3 };
	int a6 { 3 };
    return 0;
}
~~~
 注释：
+ a3、a4 使用了新的初始化方式，初始化列表来初始化对象，效果如同 a1 的直接初始化。（a3 虽然使用了等于号，但它仍然是列表初始化，私有的拷贝构造并不会影响到它，仅调用类的构造函数而不需要拷贝构造函数） 
+ a5、a6 则是基本数据类型的列表初始化方式
+ a4 和 a6 的写法，是 C++98/03 所不具备的。在 C++11 中，可以直接在变量名后面跟上初始化列表，来进行对象的初始化
+ 在C++11中，列表初始化不仅能完成对普通类型的初始化，还能完成对类的列表初始化
+  {}前面的等于号是否书写对初始化行为没有影响   

**示例2:**
~~~cpp
int* a = new int { 123 };
double b = double { 12.12 };
int* arr = new int[3] { 1, 2, 3 };
~~~
new 操作符等可以用圆括号进行初始化的地方，也可以使用初始化列表
C++11中可以使用列表初始化方法对堆中分配的内存的数组进行初始化，而在C++98/03中是不能这样做的

**示例3：**
~~~cpp
struct Foo
{
    Foo(int, double) {}
};
Foo func(void)
{
    return { 123, 321.0 };
}
~~~
列表初始化还可以直接使用在函数的返回值 
 

## 3. 列表初始化规则和细节

   首先需要了解一个概念： **聚合类型**。

   因为对于聚合类型，是可以直接使用列表初始化的。那么什么是聚会类型呢？包括两类：
1. 类型是一个普通数组，如int[5]，char[]，double[]等
2. 类型是一个类，且满足以下条件：
> 没有用户声明的构造函数
没有用户提供的构造函数(允许显示预置或弃置的构造函数)
没有私有或保护的非静态数据成员
没有基类
没有虚函数
没有{}和=直接初始化的非静态数据成员
没有默认成员初始化器

~~~cpp
struct A {
int a;
   int b;
   virtual void func() {} // 含有虚函数，不是聚合类
};

struct Base {};
struct B : public Base { // 有基类，不是聚合类
int a;
   int b;
};

struct C {
   int a;
   int b = 10; // 有等号初始化，不是聚合类
};

struct D {
   int a;
   int b;
private:
   int c; // 含有私有的非静态数据成员，不是聚合类
};

struct E {
int a;
   int b;
   E() : a(0), b(0) {} // 含有默认成员初始化器，不是聚合类
};
~~~
 
**对于一个聚合类型，使用列表初始化相当于对其中的每个元素分别赋值；对于非聚合类型，需要先自定义一个对应的构造函数，此时列表初始化将调用相应的构造函数。**

 接下来，分析下几个特例情况：

（1）存在用户自定义的构造函数的情况
~~~cpp
struct Foo
{
	int x;
	int y;
	Foo(int, int){ cout << "Foo construction"; }
};
 
int main()
{
	Foo foo{ 123, 321 };
	cout << foo.x << " " << foo.y;
	return 0;

}
~~~
输出结果为：Foo construction -858993460 -858993460
可以看出对于有用户自定义构造函数的类使用初始化列表其成员初始化后变量值是一个随机值，因此用户必须以用户自定义构造函数来构造对象。 
~~~cpp
struct Foo
{
	int x;
	int y;
	Foo(int a, int b):x{a}, y{b} { cout << "Foo construction"; }
};
 
int main()
{
	Foo foo{ 123, 321 };
	cout << foo.x << " " << foo.y;
	return 0;

}
~~~
输出结果为：Foo construction 123 321

(2)类包含有私有的或者受保护的非静态数据成员的情况
~~~cpp
struct Foo
{
	int x;
	int y;
	
protected:
	double z;
};
 
int main()
{
	Foo foo{ 123,456,789.0 };
	cout << foo.x << " " << foo.y;
	return 0;
}

error C2440: "初始化“：无法从”initializer list" 转换为“Foo”
~~~

（3）类含有基类或者虚函数
~~~cpp
struct Foo
{
	int x;
	int y;
	virtual void func(){};
};
 
int main
{
	Foo foo {123,456};
	cout << foo.x << " " << foo.y;
	return 0;

}

error C2440: "初始化“：无法从”initializer list" 转换为“Foo”
~~~

## 4. std::initializer_list<T>
我们平时开发使用STL过程中可能发现它的初始化列表可以是任意长度.
~~~cpp
int arr[] = { 1, 2, 3, 4, 5 };
std::map < int, int > map_t { { 1, 2 }, { 3, 4 }, { 5, 6 }, { 7, 8 } };
std::list<std::string> list_str{ "hello", "world", "china" };
std::vector<double> vec_d { 0.0,0.1,0.2,0.3,0.4,0.5};
~~~
大家有没有想过它是怎么实现的呢，答案是std::initializer_list，看下面这段示例代码 
~~~cpp
struct CustomVec {
   std::vector<int> data;
   CustomVec(std::initializer_list<int> list) {
       for (auto iter = list.begin(); iter != list.end(); ++iter) {
           data.push_back(*iter);
      }
  }
};
~~~
实际上之所以STL容易拥有这种可以用任意长度的同类型数据进行初始化能力是因为STL中的容器使用了std::initialzer-list这个轻量级的类模板，std::initialzer-list可以接受任意长度的同类型的数据也就是接受可变长参数{...}.

利用这个来改写我们的Foo类: 
~~~cpp
struct Foo
{
	int x;
	int y;
	int z;
	Foo(std::initializer_list<int> list)
	{
		auto it= list.begin();
		x = *it++;
		y = *it++;
		z = *it++;
	}
};
 
int _tmain(int argc, _TCHAR* argv[])
{
	Foo foo1 {123,456,789};
	Foo foo2 { 123, 456};
	Foo foo3{ 123};
	Foo foo4{ 123, 456, 789,258 };
	cout << foo1.x << " " << foo1.y << " " << foo1.z<<endl;
	cout << foo2.x << " " << foo2.y << " " << foo2.z << endl;
	cout << foo3.x << " " << foo3.y << " " << foo3.z << endl;
	cout << foo4.x << " " << foo4.y << " " << foo4.z << endl;
	return 0;
}
~~~
程序的输出结果为：
~~~cpp
123 456 789
123 456 -858993460
123 -858993460 -858993460
123 456 789
~~~
 由程序的输出结果可知，对于这种拥有固定数目的数据成员来说使用std::initialzer-list来改写后，如果列表初始化的参数刚好是3个则数据成员完全初始化，如果列表初始化的个数小于3个则未给予的值是一个随机值，而大于3个的话超出的列表初始化参数将被抛弃。虽然std::initialzer-list可以改写我们自定义的类，但是对于用于固定的数据成员的类来说这种改写意义不大，若列表初始化个数少于数据成员个数则会导致某些数据成员未被初始化，容易引起程序出BUG.所以，这种固定数据成员的就不太适用，因此在以后碰到需要使用可变长度的同类型的数据时，可以考虑使用std::initialzer-list。

 initializer_list是C++11提供的一种新类型，其定义于头文件<initializer_list>中，

它提供的操作如下:
~~~cpp
initializer_list<T> lst; //默认初始化；T类型元素的空列表
initializer_list<T> lst{a,b,c...};//lst的元素数量和初始值一样多；lst的元素是对应初始值的副本
lst2(lst)   
lst2=lst  //拷贝或赋值一个initializer_list对象不会拷贝列表中的元素；拷贝后，原始列表和副本元素共享
lst.size()  //列表中的元素数量
lst.begin()  //返回指向lst中首元素的指针
lst.end()   //返回指向lst中尾元素下一位置的指针
~~~
 需要注意的是,initializer_list对象中的元素永远是常量值，我们无法改变initializer_list对象中元素的值。并且，拷贝或赋值一个initializer_list对象不会拷贝列表中的元素，其实只是引用而已，原始列表和副本共享元素。

 

## 5. 列表初始化的好处
方便，且基本上可以替代括号初始化

可以使用初始化列表接受任意长度

可以防止类型窄化，避免精度丢失的隐式类型转换

什么是类型窄化，列表初始化通过禁止下列转换，对隐式转化加以限制：

从浮点类型到整数类型的转换

从 long double 到 double 或 float 的转换，以及从 double 到 float 的转换，除非源是常量表达式且不发生溢出

从整数类型到浮点类型的转换，除非源是其值能完全存储于目标类型的常量表达式

从整数或无作用域枚举类型到不能表示原类型所有值的整数类型的转换，除非源是其值能完全存储于目标类型的常量表达式

