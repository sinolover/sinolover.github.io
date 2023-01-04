# 引言

朋友突然问我，pthread和std::thread有什么区别？我不知道他为什么会问到这个问题，但是看到问题后我的第一反应是，你是不是C和C++混学的。

# 区别

1. `pthread`是`linux下的多线程API`，头文件为`pthread.h`，关于linux线程和进程区别本篇文章略过，因为本文算是给自学人的科普。  
2. std::thread是c++标准库中的线程库，其意义在于可以跨平台不改代码  
因为在`windows`下创建线程一般会用`_beginthreadex`，如果你不想为了`linux和windows`同时写2份代码，我相信大多数人的选择都是`std::thread`。  
3. 这就像是`windows`下的`Interlocked`和`linux`下的`__sync`系列函数，c++标准库中同样有实现好的`Atomic`类。  
其实区别就在于一个使用C语言且是系统公开的API函数，一个是C++标准库中封装的方法，就这么点区别。

# std::thread 如何传递对象

举个栗子：  
`Worker *woker = new Worker();`  
如果使用`std::thread`要用到的是`worker中的方法`，且这个方法并`不是static方法`,怎么办呢？  
关于c++传参问题，如果你不想引起歧义，尽量使用花括号{}，原因在这： [Most vexing parse(最烦人的解析)](#parse)

下面的栗子我们可以看见，新建了一个线程调用`worker`对象的`test方法`，但是此时这个方法并`不是static`类型，也就是我们不光要`传入方法地址`，还要传入`worker指针`，`test`方法的参数接受一个`Number的引用`。

```cpp
#include <iostream>
#include <thread>

class Number {
private:
	int num;
public:
	Number() { num = 1; };
	explicit Number(int a) :num(a) {}
	int getNum() {
		return this->num;
	}
};

class Worker {
public:
	void test(Number& number) {
		std::cout << number.getNum() << std::endl;
	}
};

int main()
{
	Number* number = new Number(99);
	Worker* worker = new Worker();

	std::thread{ &Worker::test, worker, std::ref(*number) }.join();
	
	delete number;
	delete worker;
	return 0;
}
```
test方法明明只有一个参数，为什么我们却在thread第二个参数中传递的是worker指针而不是number的引用？
> 从代码层面来讲，你可以理解为只有方法地址还不行，还需要传递一个对象指针来确保调用当前类方法时this指针是正确的。
从汇编层面来解释，调用一个非静态类的方法，在进入该类后，其ebp就是对象本身。传递的第一个参数是对象指针的原因就是为了在线程调用这个方法时知道this是谁。
关于std::ref如果你看过之前的左值右值，你还可以去了解一下完美转发。


**<div id="parse">Most vexing parse(最烦人的解析)</div>**

# 引言

Most vexing parse 是 effective c++ 书中写到的，写本文是为了讲清这到底是什么.

# 错误的方式

```cpp
#include <iostream>
class A {  
  public:
     A(const std::string& name){
        std::cout << name << std::endl;
     }
};

int main()  
{
  	char szTemp[] = "test";
	A a(std::string(szTemp));
	//A a(std::string())//同样没有输出
    return 0;
}
```
> 上面写了一个简单的类，构造函数允许传入一个string类型的引用，然后输出。  
> 在`main`函数中我们的`本意是创建一个A类型的对象`，并且传入`szTemp的string`，让其在`构造函数中输出`。  
> 但是却发现什么也没有输出，这是什么原因呢？


# Most vexing parse 解释

在C++中，如果出现 `T1 t1Name(T2(t2Name))`，C++会将它解释成`T1 t1Name(T2 t2Name)`函数声明。

拿上面弄得栗子讲：  
`A a(std::string(szTemp))`  
被解释成了 `A a(std::string szTemp)`的函数声明。

如果我们将`A a(std::string(szTemp))`替换成`A a(std::string("test"))`,或者将`A a(std::string())`替换成`A a(std::string(""))`就不会有任何问题。

## 证明

通过打印 `std::cout << typeid(a).name();` 可以发现类型根本不是`class a`，而是一个`function.`  
下面的代码我们将`A a(std::string(szTemp))`真正定义出来。

```cpp
#include <iostream>
class A {  
  public:
     A(const std::string& name){
        std::cout << name << std::endl;
     }
};

char szTemp[] = "test";

A a(std::string(szTemp)){
	//这里的szTemp并不是全局变量的szTemp
	//而是函数形参，还记得上面说过
	//被解释成了 A a(string szTemp)
	std::cout << szTemp + " 新的输出" << std::endl;
	
	return A(szTemp); 
}

int main()  
{
	//这里的a是function,会返回一个A类型的对象
	//所以不存在T1 t1name,T2 t2name 同时出现的问题。这是一个赋值语句。
    char test[]="haha";
	A obj = a(std::string(test));
    return 0;
}
```

> 输出结果：  
> test 新的输出  
> test

# 如何避免

使用C++的方式来传参。

> 1.  A a{ std::string(szTemp) };

> 2.  先创建string字符串  
>     std::string str(szTemp) ;  
>     A a(str);

> 3.  直接使用常量  
>     A a(std::string(“test”) );  
>     这样仅仅是T1 t1name(T2(常量))

> 只要避免T1 t1name T2 t2name 同时出现即可。