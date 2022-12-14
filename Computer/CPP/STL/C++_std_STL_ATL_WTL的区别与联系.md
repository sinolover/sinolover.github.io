0\. Standard Library

C++标准库，C++ Standard Library，是[类库](http://baike.baidu.com/view/185312.htm)和[函数](http://baike.baidu.com/view/15061.htm)的集合，其使用核心语言写成，由c++标准委员会制定，并不断维护更新。

简单的来说除了支持文件之外C++   标准库主要包含了三个部分：C   标准库的   C++   版本；C++   IO   库；C++   STL 

C++标准库的内容基本可以分以下为10类。

C1. 标准库中与语言支持功能相关的头文件
### C1. 标准库中与语言支持功能相关的头文件

| 头文件 | 描述 |
| --- | --- |
| <**cstddef**\> | 定义宏NULL和offsetof，以及其他标准类型size\_t和ptrdiff\_t。与对应的标准C头文件的区别是，NULL是C++空指针常量的补充定义(c++11中已有关键字nullptr），宏offsetof接受结构或者联合类型参数，只要他们没有成员指针类型的非静态成员即可。（c++11：)nullptr\_t是nullptr的类型。 |
| <**limits**\> | 提供与基本数据类型相关的定义。例如，对于每个数值数据类型，它定义了可以表示出来的最大值和最小值以及二进制数字的位数。 |
| <**climits**\> | 提供与基本整数数据类型相关的C样式定义。这些信息的C++样式定义在<limits>中 |
| <**cfloat**\> | 提供与基本浮点型数据类型相关的C样式定义。这些信息的C++样式定义在<limits>中 |
| <**cstdlib**\> | 提供支持程序启动和终止的宏和函数。这个头文件还声明了许多其他杂项函数，例如搜索和排序函数，从字符串转换为数值等函数。它与对应的标准C头文件stdlib.h不同，定义了abort(void)。abort()函数还有额外的功能，它不为静态或自动对象调用析构函数，也不调用传给atexit()函数的函数。它还定义了exit()函数的额外功能，可以释放静态对象，以注册的逆序调用用atexit()注册的函数。清除并关闭所有打开的C流，把控制权返回给主机环境。 |
| <**new**\> | 支持动态内存分配 |
| <**typeinfo**\> | 支持变量在运行期间的类型标识 |
| <**exception**\> | 支持异常处理，这是处理程序中可能发生的错误的一种方式 |
| <**cstdarg**\> | 支持接受数量可变的参数的函数。即在调用函数时，可以给函数传送数量不等的数据项。它定义了宏va\_arg、va\_end、va\_start以及va\_list类型 |
| <**csetjmp**\> | 为C样式的非本地跳跃提供函数。这些函数在C++中不常用 |
| <**csignal**\> | 为中断处理提供C样式支持 |

### C2. 支持流输入/输出的头文件

| <br> | <br> |
| --- | --- |
| 头文件 | 描述 |
| <**iostream**\> | 支持标准流cin、cout、cerr和clog的输入和输出，它还支持多字节字符标准流wcin、wcout、wcerr和wclog |
| <**iomanip**\> | 提供操纵程序，允许改变流的状态，从而改变输出的格式 |
| <**ios**\> | 定义iostream的基类 |
| <**istream**\> | 为管理输出流缓存区的输入定义模板类 |
| <**ostream**\> | 为管理输出流缓存区的输出定义模板类 |
| <**sstream**\> | 支持字符串的流输入输出 |
| <**fstream**\> | 支持文件的流输入输出 |
| <**iosfwd**\> | 为输入输出对象提供向前的声明 |
| <**streambuf**\> | 支持流输入和输出的缓存 |
| <**cstdio**\> | 为标准流提供C样式的输入和输出 |
| <**cwchar**\> | 支持多字节字符的C样式输入输出 |

### C3. 与诊断功能相关的头文件

| <br> | <br> |
| --- | --- |
| 头文件 | 描述 |
| <**stdexcept**\> | 定义标准异常。异常是处理错误的方式 |
| <**cassert**\> | 定义断言宏，用于检查运行期间的情形 |
| <**cerrno**\> | 支持C样式的错误信息 |

### C4. 定义工具函数的头文件

| <br> | <br> |
| --- | --- |
| 头文件 | 描述 |
| <**utility**\> | 定义重载的关系运算符，简化关系运算符的写入，它还定义了pair类型，该类型是一种模板类型，可以存储一对值。这些功能在库的其他地方使用 |
| <**functional**\> | 定义了许多函数对象类型和支持函数对象的功能，函数对象是支持operator()()函数调用运算符的任意对象 |
| <**memory**\> | 给容器、管理内存的函数和auto\_ptr模板类定义标准内存分配器（c++11中增加shared\_ptr与unique\_ptr，分别支持共享与独享的动态内存分配） |
| <**ctime**\> | 支持系统时钟函数 |

### C5. 支持字符串处理的头文件

| <br> | <br> |
| --- | --- |
| 头文件 | 描述 |
| <**string**\> | 为字符串类型提供支持和定义，包括单字节字符串(由char组成)的string和多字节字符串(由wchar\_t组成) |
| <**cctype**\> | 单字节字符类别 |
| <**cwctype**\> | 多字节字符类别 |
| <**cstring**\> | 为处理非空字节序列和内存块提供函数。这不同于对应的标准C库头文件，几个C样式字符串的一般C库函数被返回值为const和非const的函数对替代了 |
| <**cwchar**\> | 为处理、执行I/O和转换多字节字符序列提供函数，这不同于对应的标准C库头文件，几个多字节C样式字符串操作的一般C库函数被返回值为const和非const的函数对替代了。 |
| <**cstdlib**\> | 为把单字节字符串转换为数值、在多字节字符和多字节字符串之间转换提供函数 |

### C6. 定义容器类的模板的头文件

| <br> | <br> |
| --- | --- |
| 头文件 | 描述 |
| <**vector**\> | 定义vector序列模板，这是一个大小可以重新设置的数组类型，比普通数组更安全、更灵活 |
| <**list**\> | 定义list序列模板，这是一个序列的链表，常常在任意位置插入和删除元素 |
| <**deque**\> | 定义deque序列模板，支持在开始和结尾的高效插入和删除操作 |
| <**queue**\> | 为队列(先进先出)数据结构定义序列适配器queue和priority\_queue |
| <**stack**\> | 为堆栈(后进先出)数据结构定义序列适配器stack |
| <**map**\> | map是一个关联容器类型，允许根据键值是唯一的，且按照升序存储。multimap类似于map，但键不是唯一的 |
| <**set**\> | set是一个关联容器类型，用于以升序方式存储唯一值。multiset类似于set，但是值不必是唯一的。 |
| <**bitset**\> | 为固定长度的位序列定义bitset模板，它可以看作固定长度的紧凑型bool数组 |
| <**array**\> | （TR1）固定大小数组，支持复制 |
| <**forward\_list**\> | （c++11）单向列表，支持快速随机访问 |
| <**unordered\_set**\> | （TR1)无序容器set，其元素随机存放。multiset类似于set，但是值不必是唯一的 |
| <**unordered\_map**\> | （TR1)无序容器map，其键值随机存放。multimap类似于map，但键不是唯一的 |

### C7. 支持迭代器的头文件

| <br> | <br> |
| --- | --- |
| 头文件 | 描述 |
| <**iterator**\> | 给迭代器提供定义和支持 |

### C8. 有关算法的头文件

| <br> | <br> |
| --- | --- |
| 头文件 | 描述 |
| <**algorithm**\> | 提供一组基于算法的函数，包括置换、排序、合并和搜索 |
| <**cstdlib**\> | 声明C标准库函数bsearch()和qsort()，进行搜索和排序 |
| <**ciso646**\> | 允许在代码中使用and代替&& |

### C9. 有关数值操作的头文件

| <br> | <br> |
| --- | --- |
| 头文件 | 描述 |
| <**complex**\> | 支持复杂数值的定义和操作 |
| <**valarray**\> | 支持数值矢量的操作 |
| <**numeric**\> | 在数值序列上定义一组一般数学操作，例如accumulate和inner\_product |
| <**cmath**\> | 这是C数学库，其中还附加了重载函数，以支持C++约定 |
| <**cstdlib**\> | 提供的函数可以提取整数的绝对值，对整数进行取余数操作 |

### C10. 有关本地化的头文件

|      <br>      |                      <br>                       |
| -------------- | ----------------------------------------------- |
| 头文件          | 描述                                            |
| <**locale**\>  | 提供的本地化包括字符类别、排序序列以及货币和日期表示 |
| <**clocale**\> | 对本地化提供C样式支持                             |




1\. std

std命名空间是C++中标准库类型对象的命名空间。

在标准C++以前，都是用#include这样的写法的，因为要包含进来的头文件名就是iostream.h。标准C++引入了名字空间的概念，并把iostream等标准库中的东东封装到了std名字空间中，同时为了不与原来的头文件混淆，规定标准C++使用一套新的头文件，这套头文件的文件名后不加.h扩展名，如iostream、string等等，并且把原来C标准库的头文件也重新命名，如原来的string.h 就改成cstring(就是把.h去掉，前面加上字母c)，所以头文件包含的写法也就变成了#include 。 

并不是写了#include就必须用using namespace std;我们通常这样的写的原因是为了一下子把std名字空间的东东全部暴露到全局域中（就像是直接包含了iostream.h这种没有名字空间的头文件一样），使标准C++库用起来与传统的iostream.h一样方便，但并不建议这样做，因为使用using namespace std;的话就没有起到命名空间的作用。再次回到了如同没有涉及命名空间时，所有标示符都定义在全局作用于中的混乱情况，不利于程序员创建新对象。

如果不用using namespace std;使用标准库时就得时时带上名字空间的全名，如std::cout << "hello" << std::endl; 

 和是不一样，前者没有后缀，实际上，在编译器include文件夹里面可以看到，二者是两个文件，打开文件就会发现，里面的代码是不一样的。后缀为.h的头文件c++标准已经明确提出不支持了，早些的实现将标准库功能定义在全局空间里，声明在带.h后缀的头文件里，c++标准为了和C区别开，也为了正确使用命名空间，规定头文件不使用后缀.h。因此，当使用时，相当于在c中调用库函数，使用的是全局命名空间，也就是早期的c++实现；当使用的时候，该头文件没有定义全局命名空间，必须使用namespace std；这样才能正确使用cout。

使用标准库中标示符的方法

*   使用标示符限定命名空间：std::cout<<"Hello!"<
*   使用using std::cout;事先声明：cout<<"Hello!"<哪个，保证程序中名称的唯一性
*   使用using naspace std声明：cout<<"Hello!"<

来源： <[http://blog.csdn.net/miss\_acha/article/details/7233741](http://blog.csdn.net/miss_acha/article/details/7233741)\>

/

2\. STL

STL（Standard Template Library，标准模板库)是惠普实验室开发的一系列软件的统称。现然主要出现在C++中，但在被引入C++之前该技术就已经存在了很长的一段时间。

一、STL简介

（一）、泛型程序设计

泛型编程（generic programming）

将程序写得尽可能通用

将算法从数据结构中抽象出来，成为通用的

C++的模板为泛型程序设计奠定了关键的基础

（二）、什么是STL

1、STL（Standard Template Library），即标准模板库，是一个高效的C++程序库。

2、包含了诸多在计算机科学领域里常用的基本数据结构和基本算法。为广大C++程序员们提供了一个可扩展的应用框架，高度体现了软件的可复用性

3、从逻辑层次来看，在STL中体现了泛型化程序设计的思想（generic programming）。在这种思想里，大部分基本算法被抽象，被泛化，独立于与之对应的数据结构，用于以相同或相近的方式处理各种不同情形。

4、从实现层次看，整个STL是以一种类型参数化（type parameterized）的方式实现的基于模板(template)

二、STL组件

1.  Container(容器) 各种基本数据结构
2.  Adapter(适配器) 可改变containers、Iterators或Function object接口的一种组件
3.  Algorithm(算法) 各种基本算法如sort、search…等
4.  Iterator(迭代器) 连接containers和algorithms
5.  Function object(函数对象) 
6.  Allocator(分配器)

（一）、容器

容器类是容纳、包含一组元素或元素集合的对象

七种基本容器：

1.  向量（vector）
2.  双端队列（deque）
3.  列表（list）
4.  集合（set）
5.  多重集合（multiset）
6.  映射（map）
7.  多重映射（multimap）

标准容器的成员绝大部分都具有共同的名称

序列式容器

序列式容器Sequence containers,其中每个元素均有固定位置——取决于插入时机和地点，和元素值无关。（vector、deque、list）

关联式容器

关联式容器Associative containers，元素位置取决于特定的排序准则以及元素值，和插入次序无关。（set、multiset、map、multimap）

1、需要频繁在序列中间位置上进行插入和/或删除操作且不需要过多地在序列内部进行长距离跳转，应该选择list

2、vector头部与中间插入删除效率较低，在尾部插入与删除效率高。

3、deque是在头部与尾部插入与删除效率较高

set/map/multiset/multimap （摘自 [http://blog.csdn.net/v\_july\_v/article/details/7382693](http://blog.csdn.net/v_july_v/article/details/7382693)）

 set，同map一样，所有元素都会根据元素的键值自动被排序，因为set/map两者的所有各种操作，都只是转而调用RB\-tree的操作行为，不过，值得注意的是，两者都不允许两个元素有相同的键值。

 不同的是：set的元素不像map那样可以同时拥有实值(value)和键值(key)，set元素实值就是键值，键值就是实值，而map的所有元素都是pair，同时拥有实值(value)和键值(key)，pair的第一个元素被视为键值，第二个元素被视为实值。

 至于multiset/multimap，他们的特性及用法和set/map完全相同，唯一的差别就在于它们允许键值重复，即所有的插入操作基于RB-tree的insert\_equal()而非insert\_unique()。In computer science, a multimap (sometimes also multihash) is a generalization of a map or associative array

abstract data type in which more than one value may be associated with and returned for a given key.

hash\_set/hash\_map/hash\_multiset/hash\_multimap

 hash\_set/hash\_map，两者的一切操作都是基于hashtable之上。不同的是，hash\_set同set一样，同时拥有实值和键值，且实值就是键值，键值就是实值，而hash\_map同map一样，每一个元素同时拥有一个实值(value)和一个键值(key)，所以其使用方式，和上面的map基本相同。但由于hash\_set/hash\_map都是基于hashtable之上，所以不具备自动排序功能。为什么? 因为hashtable没有自动排序功能。

 至于hash\_multiset/hash\_multimap的特性与上面的multiset/multimap完全相同，唯一的差别就是它们hash\_multiset/hash\_multimap的底层实现机制是hashtable(而multiset/multimap，上面说了，底层实现机制是RB\-tree)，所以它们的元素都不会被自动排序，不过也都允许键值重复。

 所以，综上，说白了，什么样的结构决定其什么样的性质，因为set/map/multiset/multimap都是基于RB-tree之上，所以有自动排序功能，而hash\_set/hash\_map/hash\_multiset/hash\_multimap都是基于hashtable之上，所以不含有自动排序功能，至于加个前缀multi\_无非就是允许键值重复而已。

（二）、迭代器

1、迭代器Iterators，用来在一个对象群集（collection of objects）的元素上进行遍历。这个对象群集或许是个容器，或许是容器的一部分。迭代器的主要好处是，为所有容器提供了一组很小的公共接口。迭代器以++进行累进，以\*进行提领，因而它类似于指针，我们可以把它视为一种smart pointer

2、比如++操作可以遍历至群集内的下一个元素。至于如何做到，取决于容器内部的数据组织形式。

3、每种容器都提供了自己的迭代器，而这些迭代器能够了解容器内部的数据结构。

（三）、算法

算法Algorithms，用来处理群集内的元素。它们可以出于不同的目的而搜寻、排序、修改、使用那些元素。通过迭代器的协助，我们可以只需编写一次算法，就可以将它应用于任意容器，这是因为所有的容器迭代器都提供一致的接口。

（四）、适配器

1、适配器是一种接口类

为已有的类提供新的接口

目的是简化、约束、使之安全、隐藏或者改变被修改类提供的服务集合

2、三种类型的适配器：

1.  容器适配器：用来扩展7种基本容器，它们和顺序容器相结合构成栈、队列和优先队列容器
2.  迭代器适配器（反向迭代器、插入迭代器、IO流迭代器）
3.  函数适配器（函数对象适配器、成员函数适配器、普通函数适配器）

（五）、函数对象

1、函数对象（function object）也称为仿函数（functor）

2、一个行为类似函数的对象，它可以没有参数，也可以带有若干参数。

3、任何重载了调用运算符operator()的类的对象都满足函数对象的特征

4、函数对象可以把它称之为smart function。

5、STL中也定义了一些标准的函数对象，如果以功能划分，可以分为算术运算、关系运算、逻辑运算三大类。为了调用这些标准函数对象，需要包含头文件。

（六）、分配器（摘自 http://blog.csdn.net/hackbuteer1/article/details/7724534）

负责空间配置与管理。从实现的角度来看，配置器是一个实现了动态空间配置、空间管理、空间释放的class template。

隐藏在这些容器后的内存管理工作是通过STL提供的一个默认的allocator实现的。当然，用户也可以定制自己的allocator，只要实现allocator模板所定义的接口方法即可，然后通过将自定义的allocator作为模板参数传递给STL容器，创建一个使用自定义allocator的STL容器对象，如：

 stl::vector array;

 大多数情况下，STL默认的allocator就已经足够了。这个allocator是一个由两级分配器构成的内存管理器，当申请的内存大小大于128byte时，就启动第一级分配器通过malloc直接向系统的堆空间分配，如果申请的内存大小小于128byte时，就启动第二级分配器，从一个预先分配好的内存池中取一块内存交付给用户，这个内存池由16个不同大小（8的倍数，8~128byte）的空闲列表组成，allocator会根据申请内存的大小（将这个大小round up成8的倍数）从对应的空闲块列表取表头块给用户。

这种做法有两个优点：

 （1)小对象的快速分配。小对象是从内存池分配的，这个内存池是系统调用一次malloc分配一块足够大的区域给程序备用，当内存池耗尽时再向系统申请一块新的区域，整个过程类似于批发和零售，起先是由allocator向总经商批发一定量的货物，然后零售给用户，与每次都总经商要一个货物再零售给用户的过程相比，显然是快捷了。当然，这里的一个问题时，内存池会带来一些内存的浪费，比如当只需分配一个小对象时，为了这个小对象可能要申请一大块的内存池，但这个浪费还是值得的，况且这种情况在实际应用中也并不多见。

 (2)避免了内存碎片的生成。程序中的小对象的分配极易造成内存碎片，给操作系统的内存管理带来了很大压力，系统中碎片的增多不但会影响内存分配的速度，而且会极大地降低内存的利用率。以内存池组织小对象的内存，从系统的角度看，只是一大块内存池，看不到小对象内存的分配和释放。

来源： <[http://m.blog.csdn.net/blog/Simba888888/9410459](http://m.blog.csdn.net/blog/Simba888888/9410459)\>

/

3\. ATL

ATL，Active Template Library活动模板库，是一种微软程序库，支持利用C++语言编写ASP代码以及其它ActiveX程序。通过活动模板库，可以建立COM组件，然后通过ASP页面中的脚本对COM对象进行调用。这种COM组件可以包含属性页、对话框等控件。 

ATL被包含在Visual Studio中，作为IDE的一部分。

ATL是ActiveX Template Library 的缩写，它是一套C++模板库。使用ATL能够快速地开发出高效、简洁的代码（Effective and Slim code），同时对COM组件的开发提供最大限度的代码自动生成以及可视化支持。为了方便使用，从Microsoft[Visual C++](http://baike.baidu.com/view/100377.htm)5.0版本开始，Microsoft把ATL集成到Visual C++[开发环境](http://baike.baidu.com/view/4831305.htm)中。1998年9月推出的Visual Studio 6.0 集成了ATL 3.0版本。ATL已经成为Microsoft标准开发工具中的一个重要成员，日益受到C++开发人员的重视。

首先ATL的基本目标就是使COM应用开发尽可能地自动化，这个基本目标就决定了ATL只面向COM开发提供支持。目标的明确使ATL对COM技术的支持达到淋漓尽致的地步。对COM开发的任何一个环节和过程，ATL都提供支持，并将与COM开发相关的众多工具集成到一个统一的编程环境中。对于COM/ActiveX的各种应用，ATL也都提供了完善的Wizard支持。所有这些都极大地方便了开发者的使用，使开发者能够把注意力集中在与应用本身相关的逻辑上。

其次，ATL因其采用了特定的基本实现技术，摆脱了大量冗余代码，使用ATL开发出来的COM应用的代码简练高效，即所谓的“Slim Code”。ATL在实现上尽可能采用优化技术，甚至在其内部提供了所有C/C++开发的程序所必须具有的C启动代码的替代部分。同时ATL产生的代码在运行时不需要依赖于类似MFC程序所需要的庞大的代码模块，包含在最终模块中的功能是用户认为最基本和最必须的。这些措施使采用ATL开发的COM组件（包括ActiveX Control)可以在网络环境下实现应用的分布式组件结构。

第三，ATL的各个版本对Microsoft的基于COM的各种新的组件技术如MTS、ASP等都有很好的支持，ATL对新技术的反应速度大大快于MFC。ATL已经成为Microsoft支持COM应用开发的主要开发工具，因此COM技术方面的新进展在很短的时间内都会在ATL中得到反映。这使开发者使用ATL进行COM编程可以得到直接使用COM SDK编程同样的灵活性和强大的功能。

来源： <[http://baike.baidu.com/link?url=Pjx1EywQQcaaIYKIPydO8f-wfb2AJv-OOdqbTz5gry1ai0K\_CWHDm4v7QaobnU\_2qUCgLjpV2AVSvA1GJpOd9HYj9kYWbaNC7C0QxS5bmPW](http://baike.baidu.com/link?url=Pjx1EywQQcaaIYKIPydO8f-wfb2AJv-OOdqbTz5gry1ai0K_CWHDm4v7QaobnU_2qUCgLjpV2AVSvA1GJpOd9HYj9kYWbaNC7C0QxS5bmPW)\>

ATL是一个产生C++/COM代码的框架，就如同C语言是一个产生汇编代码的框架。ATL又不同于MFC，它完全面向COM组件，其技术路线也不同于MFC，MFC使用的是C++中的继承、封装、嵌套等常规技术，而ATL使用了C++中模板、多继承等高级技术，甚至还用到了STL。所以学习和使用ATL要求我们必须熟悉这些C++高级特性。另一方面，ATL结构完全针对COM中的诸多规范，这就要求使用人员必须非常了解COM规范，才有可能真正把ATL用好。

 对于COM应用的开发，ATL无疑是首选的工具，与MFC相比，ATL的规模还不算大，但是从上述的介绍我们可以看出，ATL涉及到了COM的方方面面。实际上，ATL的内容还要多得多，比如OLEDB的支持、MTS的支持等。尽管如此，如果我们有了《ATL Internals》这本书中的内容为基础，那么再去学习这些扩展的内容就会容易得多，结合ATL中实现COM的基本手段加上这些应用技术的背景知识，我们可以很容易地掌握这些开发技术。 

 但是如果我们要想熟练掌握甚至精通ATL的话，那么这只是一个开头，还有漫长的路要走。原因有多方面，一是COM本身异常复杂，不下苦功难窥全貌；二是ATL确实奥妙很多，它体现了C++语法的博大精深；三是ATL还存在很多错误，虽然本书作者指出了一些错误，但实际的错误肯定更多，这就对ATL使用者提出了更高的要求，如果使用过程中不能发现这些错误或者避开这些错误，那么用ATL反而会阻碍我们的工作。 

 虽然ATL比较精深，但是这本书的讲解非常通俗易懂，语言比较简练，条理非常清楚。即使在读完这本书之后，它仍然可以作为参考书指导我们的开发和学习工作。我想，这就是好书的价值所在吧。 

来源： <[http://blog.csdn.net/wangwenjing90/article/details/8925382](http://blog.csdn.net/wangwenjing90/article/details/8925382)\>

 简单地说来，ATL中所使用的基本技术包括以下几个方面：

1.  COM技术
2.  C++[模板类](http://baike.baidu.com/view/1923683.htm)技术（Template)
3.  C++[多继承](http://baike.baidu.com/view/1160382.htm)技术（Multi-Inheritance)

COM技术是理解ATL的基础，使用ATL进行开发要对COM技术的基本概念有最低限度的了解。由于COM是一项非常复杂庞大的技术体系，限于本文的篇幅，这里不再赘述。对于本文中提到的COM基本概念也不做过多的解释，请读者参阅有关的参考书籍。

作为ATL最核心的实现技术的模板是对标准C++语言的扩展，但是在大多数的C++[编程环境](http://baike.baidu.com/view/1405990.htm)中，人们很少使用它，这是因为模板的功能虽然很强，但是它内部机制比较复杂，需要比较多的C++知识和经验才能灵活地使用它。在MFC中的CObjectArray等功能类就是由模板来定义的。完全通过模板来定义程序的整体类结构，ATL是迄今为止做得最为成功的。

所谓[模板类](http://baike.baidu.com/view/1923683.htm)简单地说是对类的抽象。我们知道C++语言用类定义了构造对象（这里指C++对象而不是COM对象）的方式，对象是类的实例，而[模板类](http://baike.baidu.com/view/1923683.htm)定义的是类的构造方式，使用[模板类](http://baike.baidu.com/view/1923683.htm)定义实例化的结果产生的是不同的类。因此可以说[模板类](http://baike.baidu.com/view/1923683.htm)是“类的类”。

/

4\. WTL

 在ATL出现的时候，一些部分COM的编程人员开始觉得开发COM运用是一种快乐，因为使用它很方便地开发小规模的COM组件，但好景不长，现实的COM组件是包罗相当广泛的，特别当它们准备使用窗口控件，发现ATL提供的相当的稀少。因此Microsoft推出了半成品与没有技术支持的WTL，这也是WTL诞生的原因。 

 很多初次接触WTL都问“WTL这三个字母代表什么呢？”：WTL全称为Windows Template Library，构架于ATL之上，采用C++模板技术来包装大部窗口控制，并给出一个与MFC相似的应用框架。他们紧跟着问“那我如何得到它呢？”：由于WTL是Microsoft推出的，在Microsoft的PlatFormSDK中就有WTL是ATL的扩展，也是由ATL小组开发，包含在Microsoft于2000年1月发布的开发平台SDK包中（也可以从Microsoft网站上下载），虽然Microsoft没有正式支持。WTL通过提供一个用于编写Win32应用程序和控制的轻量级的框架，一些特殊的视图、GDI对象和实用的类，来扩展了ATL窗口类WTL设计特性--附带地，相对于MFC的优势\--包括： 

*    (1) 模板化，因此有较小的代码量。例如，一个简单的“hello world”SDI应用程序，基于WTL的程序只有24KB，而MFC静态连接结果是440KB，MFC动态连接的结果是24KB+1MB。 
*    (2) 无太多相关性，并且可以自由地和SDK代码直接混合。 
*    (3) 不会强迫使用特定的应用程序模型，尤其相对于MFC的应用程序框架。 

1\. WTL类包括： 

*    (1) 标准控制（编辑框，列表框，按钮等等） 
*    (2) 公共控制（包括列表视图，树形视图，进度条，微调按钮） 
*    (3) IE控制（rebar，平面滚动条，日历等等） 
*    (4) 命令条，菜单，和更新UI类 
*    (5) 公共对话框 
*    (6) 属性单和页类 
*    (7) 框架窗口，MDI框架和子框架，分隔条，可滚动的窗口 
*    (8) 设备环境(DC)和GDI对象类（笔、刷子、位图等） 
*    (9) 打印机及其信息和设备模式类 

2\. 实用工具类：包括CPoint, CRect, CSize, 和CString类 

 WTLAppWizard允许你生成SDI、MDI、多线程SDI和基于对话框的应用程序。多线程SDI应用程序就象IE或WindowsExplorer（我的电脑），看起来象是启动了多个实例，实质上它们是同一进程的多个视图。这些视图可以是普通的基于CWindowImpl的窗口，或基于窗体、列表框、编辑框、列表视图、树形视图、丰富文本编辑框或HTML控制。你可以让你的应用程序拥有rebar、命令条（如同WindowsCE）、工具条或状态条。你的应用程序可以包含ActiveX控制，甚至可以是一个COM服务器。 

 WTL = Windows Template Library，可以说起源于ATL 类库中关于Window 创建/管理的类。主要原因是用原始的 WIN32 API 编写漂亮的用户界面工作量大，繁杂。MFC 虽然提供了一套很好的封装，但是也不是很容易消化和使用，特别是各个 MFC 类之间耦合很紧，要用好 MFC 就要理解很多 MFC 内在的运行机制（有人说 MFC 的封装是“白盒”封装，呵呵）。WTL 利用 C++ 模版的高级功能，提供很联系很松散的“独立”的类库，使用起来比较方便，而且代码体积小，不必为了学习某个类必须学习一大堆相关的类。但是 WTL 不是 Microsoft 官方正式支持的类库，虽然有相当多的人和越来越多的在使用，不过有可能将来会支持的。

来源： <[http://blog.csdn.net/wangwenjing90/article/details/8925382](http://blog.csdn.net/wangwenjing90/article/details/8925382)\>