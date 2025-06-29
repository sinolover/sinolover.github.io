# C++著名程序库的比较和学习经验(STL.Boost.GUI.XML.网络等等)
## 0、序言
　　在C++中，库的地位是非常高的。C++之父 Bjarne Stroustrup先生多次表示了设计库来扩充功能要好过设计更多的语法的言论。现实中，C++的库门类繁多，解决的问题也是极其广泛，库从轻量级到重量级的都有。不少都是让人眼界大开，亦或是望而生叹的思维杰作。由于库的数量非常庞大，而且限于笔者水平，其中很多并不了解。所以文中所提的一些库都是比较著名的大型库。

　　这篇文章在网上有很多转载，大部分都分成几篇文章，我把它合在一起。我不清楚原文在网络上的最早出处，我找到的这篇文章的地址http://www.kuqin.com/language/20090215/34991.html，发表于2009年2月，从文章的内容看，这篇文章的成文时间远早于这个的日期。由于本人的水平很有限，文中所提到的很多都不了解，甚至都不知道，但对于本人略知一二，且原文中有明显错误的，我都尽力指出，需要说明的是，并不是原文不好，因为时间的发展，部分描述已经与目前的状态不十分符合，本人的注解只作参考，请多参考其他资料。－－ wangxinus

　　标准库中提供了C++程序的基本设施。虽然C++标准库随着C++标准折腾了许多年，直到标准的出台才正式定型，但是在标准库的实现上却很令人欣慰得看到多种实现，并且已被实践证明为有工业级别强度的佳作。

*为显示简短，故标题名从C++各大有名库的介绍 变更为  C++名库*

## 1、C++名库——C++标准库

1、Dinkumware C++ Library 参考站点：http://www.dinkumware.com/
> P.J. Plauger编写的高品质的标准库。P.J. Plauger博士是Dr. Dobb's程序设计杰出奖的获得者。其编写的库长期被Microsoft采用，并且最近Borland也取得了其OEM的license，在其 C/C++的产品中采用Dinkumware的库。

2、RogueWave Standard C++ Library 参考站点：http://www.roguewave.com/
> 这个库在Borland C++ Builder的早期版本中曾经被采用，后来被其他的库给替换了。笔者不推荐使用。

3、SGI STL 参考站点：http://www.roguewave.com/
> SGI公司的C++标准模版库。

4、STLport 参考站点：http://www.stlport.org/
> SGI STL库的跨平台可移植版本。

## 2、C++名库——准标准库Boost

Boost总体来说是实用价值很高，质量很高的库。并且由于其对跨平台的强调，对标准C++的强调，是编写平台无关，现代C++的开发者必备的 工具。但是Boost中也有很多是实验性质的东西，在实际的开发中实用需要谨慎。并且很多Boost中的库功能堪称对语言功能的扩展，其构造用尽精巧的手法，不要贸然的花费时间研读。Boost另外一面，比如Graph这样的库则是具有工业强度，结构良好，非常值得研读的精品代码，并且也可以放心的在产品代码中多多利用。
参考站点：[http://www.boost.org](http://www.boost.org)
> Boost库是一个经过千锤百炼、可移植、提供源代码的C++库，作为标准库的后备，是C++标准化进程的发动机之一。 Boost库由C++标准委员会库工作组成员发起，在C++社区中影响甚大，其成员已近2000人。 Boost库为我们带来了最新、最酷、最实用的技术，是不折不扣的“准”标准库。Boost中比较有名气的有这么几个库：
> 1. Regex 正则表达式库
> 2. Spirit LL parser framework，用C++代码直接表达EBNF
> 3. Graph 图组件和算法
> 4. Lambda 在调用的地方定义短小匿名的函数对象，很实用的functional功能
> 5. concept check 检查泛型编程中的concept
> 6. Mpl 用模板实现的元编程框架
> 7. Thread 可移植的C++多线程库
> 8. Python 把C++类和函数映射到Python之中
> 9. Pool 内存池管理
> 10. smart_ptr 5个智能指针，学习智能指针必读，一份不错的参考是来自CUJ的文章：
> 11. Smart Pointers in Boost,哦，这篇文章可以查到，CUJ是提供在线浏览的。中文版见笔者在《Dr.Dobb's Journal软件研发杂志》第7辑上的译文。


## 3、C++名库——GUI

在众多C++的库中，GUI部分的库算是比较繁荣，也比较引人注目的。在实际开发中，GUI库的选择也是非常重要的一件事情，下面我们综述一下可选择的GUI库，各自的特点以及相关工具的支持。

1、MFC
> 大名鼎鼎的微软基础类库（Microsoft Foundation Class）。大凡学过VC++的人都应该知道这个库。虽然从技术角度讲，MFC是不大漂亮的，但是它构建于Windows API 之上，能够使程序员的工作更容易,编程效率高，减少了大量在建立 Windows 程序时必须编写的代码，同时它还提供了所有一般 C++ 编程的优点，例如继承和封装。MFC 编写的程序在各个版本的Windows操作系统上是可移植的，例如，在Windows 3.1下编写的代码可以很容易地移植到 Windows NT 或 Windows 95 上。但是在最近发展以及官方支持上日渐势微。

2、WxWindows　参考网站：http://www.wxwindows.org
> 跨平台的GUI库。因为其类层次极像MFC，所以有文章介绍从MFC到WxWindows的代码移植以实现跨平台的功能。通过多年的开发也是一 个日趋完善的GUI库，支持同样不弱于前面两个库。并且是完全开放源代码的。新近的C++ Builder X的GUI设计器就是基于这个库的。\[wangxinus注:迫于微软的施压，已经由WxWindows更名为wxWidgets\]

3、QT　参考网站：http://www.trolltech.com
> Qt是Trolltech公司的一个多平台的C++图形用户界面应用程序框架。它提供给应用程序开发者建立艺术级的图形用户界面所需的所用功 能。Qt是完全面向对象的很容易扩展，并且允许真正地组件编程。自从1996年早些时候，Qt进入商业领域，它已经成为全世界范围内数千种成功的应用程序 的基础。Qt也是流行的Linux桌面环境KDE 的基础，同时它还支持Windows、Macintosh、Unix/X11等多种平台。\[wangxinus注:QT目前已经是Nokia旗下的产品，原官方网站已经失效，目前为http://qt.nokia.com.2009年初发布的Qt4.5版本开始使用LGPL协议，诺基亚希望以此来吸引更多的开发人员使用Qt库\]

4、Fox　参考网站：http://www.fox-toolkit.org/
> 开放源代码的GUI库。作者从自己亲身的开发经验中得出了一个理想的GUI库应该是什么样子的感受出发，从而开始了对这个库的开发。有兴趣的可以尝试一下。

5、WTL
> 基于ATL的一个库。因为使用了大量ATL的轻量级手法，模板等技术，在代码尺寸，以及速度优化方面做得非常到位。主要面向的使用群体是开发COM轻量级供网络下载的可视化控件的开发者。

6、GTK 参考网站：http://gtkmm.sourceforge.net/
> GTK是一个大名鼎鼎的C的开源GUI库。在Linux世界中有Gnome这样的杀手应用。而Qt就是这个库的C++封装版本。\[wangxinus注:“Qt就是这个库的C++封装版本”是错误的。Qt早于GTK，最初Qt由于协议的原因引起社区的不满，另外开发了一个基于C语言的GTK库，后面的扩展版本为GTK＋。GTK+的Gnome和Qt的KDE是目前linux桌面的两大阵营，曾有水火不容之势。目前双方都以及开源社区的精神，已经和解。\]

## 4、C++名库——网络通信

１、ACE　参考网站：http://www.cs.wustl.edu/~schmidt/ACE.html
> C++库的代表，超重量级的网络通信开发框架。ACE自适配通信环境（Adaptive Communication Environment）是可以自由使用、开放源代码的面向对象框架，在其中实现了许多用于并发通信软件的核心模式。ACE提供了一组丰富的可复用C++ 包装外观（Wrapper Facade）和框架组件，可跨越多种平台完成通用的通信软件任务，其中包括：事件多路分离和事件处理器分派、信号处理、服务初始化、进程间通信、共享内 存管理、消息路由、分布式服务动态（重）配置、并发执行和同步，等等。

２、StreamModule　参考网站：http://www.omnifarious.org/StrMod
> 设计用于简化编写分布式程序的库。尝试着使得编写处理异步行为的程序更容易，而不是用同步的外壳包起异步的本质。

３、SimpleSocket　参考网站：http://home.hetnet.nl/~lcbokkers/simsock.htm
> 这个类库让编写基于socket的客户/服务器程序更加容易。

４、A Stream Socket API for C++ 参考网站：http://www.pcs.cnu.edu/~dgame/sockets/socketsC++/sockets.html
> 又一个对Socket的封装库。

## 5、C++名库——XML
１、Xerces　参考网站：http://xml.apache.org/xerces-c/
> Xerces-C++ 是一个非常健壮的XML解析器，它提供了验证，以及SAX和DOM API。XML验证在文档类型定义(Document Type Definition，DTD)方面有很好的支持，并且在2001年12月增加了支持W3C XMLSchema 的基本完整的开放标准。

２、XMLBooster　参考网站：http://www.xmlbooster.com/
> 这个库通过产生特制的parser的办法极大的提高了XML解析的速度，并且能够产生相应的GUI程序来修改这个parser。在DOM和SAX两大主流XML解析办法之外提供了另外一个可行的解决方案。

３、Pull Parser　参考网站：http://www.extreme.indiana.edu/xgws/xsoap/xpp
> 这个库采用pull方法的parser。在每个SAX的parser底层都有一个pull的parser，这个xpp把这层暴露出来直接给大家使用。在要充分考虑速度的时候值得尝试。

４、Xalan　参考网站：http://xml.apache.org/xalan-c/
> Xalan是一个用于把XML文档转换为HTML，纯文本或者其他XML类型文档的XSLT处理器。

５、CMarkup　参考网站：http://www.firstobject.com/xml.htm
> 这是一种使用EDOM的XML解析器。在很多思路上面非常灵活实用。值得大家在DOM和SAX之外寻求一点灵感。

６、libxml++ 参考网站：[http://libxmlplusplus.sourceforge.net/](http://libxmlplusplus.sourceforge.net/)
> libxml++是对著名的libxml XML解析器的C++封装版本。

7、TinyXML
> 一个非常小巧的XML解析库，基于DOM的

## 6、C++名库——科学计算

１、Blitz++ 参考网站：http://www.oonumerics.org/blitz
> Blitz++ 是一个高效率的数值计算函数库，它的设计目的是希望建立一套既像C++ 一样方便，同时又比Fortran速度更快的数值计算环境。通常，用C++所写出的数值程序，比 Fortran慢20%左右，因此Blitz++正是要改掉这个缺点。方法是利用C++的template技术，程序执行甚至可以比Fortran更快。
Blitz++目前仍在发展中，对于常见的SVD，FFTs，QMRES等常见的线性代数方法并不提供，不过使用者可以很容易地利用Blitz++所提供的函数来构建。

２、POOMA 参考网站：http://www.codesourcery.com/pooma/pooma
> POOMA是一个免费的高性能的C++库，用于处理并行式科学计算。POOMA的面向对象设计方便了快速的程序开发，对并行机器进行了优化以达到最高的效率，方便在工业和研究环境中使用。

３、MTL 参考网站：http://www.osl.iu.edu/research/mtl
> Matrix Template Library(MTL)是一个高性能的泛型组件库，提供了各种格式矩阵的大量线性代数方面的功能。在某些应用使用高性能编译器的情况下，比如Intel的编译器，从产生的汇编代码可以看出其与手写几乎没有两样的效能。

４、CGAL 参考网站：www.cgal.org
> Computational Geometry Algorithms Library的目的是把在计算几何方面的大部分重要的解决方案和方法以C++库的形式提供给工业和学术界的用户。

## 7、C++名库——游戏开发

１、Audio/Video 3D C++ Programming Library 参考网站：http://www.galacticasoftware.com/products/av/
> AV3D是一个跨平台，高性能的C++库。主要的特性是提供3D图形，声效支持（SB,以及S3M），控制接口（键盘，鼠标和遥感），XMS。

２、KlayGE 参考网站：http://home.g365.net/enginedev/
> 国内游戏开发高手自己用C++开发的游戏引擎。KlayGE是一个开放源代码、跨平台的游戏引擎，并使用Python作脚本语言。KlayGE在LGPL协议下发行。感谢龚敏敏先生为中国游戏开发事业所做出的贡献。
> \[wangxinus注:这个库国人了解很少，百度百科的KlayGE词条还是作者本人创建的。一个人开发一个游戏引擎库，实在让笔者汗颜，对作者表示钦佩！\]

３、OGRE 参考网站：http://www.ogre3d.org
> OGRE（面向对象的图形渲染引擎）是用C++开发的，使用灵活的面向对象3D引擎。它的目的是让开发者能更方便和直接地开发基于3D硬件设备 的应用程序或游戏。引擎中的类库对更底层的系统库（如：Direct3D和OpenGL）的全部使用细节进行了抽象，并提供了基于现实世界对象的接口和其 它类。

## 8、C++名库——线程

１、C++ Threads 参考网站：http://threads.sourceforge.net/
> 这个库的目标是给程序员提供易于使用的类，这些类被继承以提供在Linux环境中很难看到的大量的线程方面的功能。

２、ZThreads 参考网站：http://zthread.sourceforge.net/
> 一个先进的面向对象，跨平台的C++线程和同步库。

## 9、C++名库——序列化

１、s11n 参考网站：http://s11n.net/
> 一个基于STL的C++库，用于序列化POD，STL容器以及用户定义的类型。

２、Simple XML Persistence Library 参考网站：http://sxp.sourceforge.net/
> 这是一个把对象序列化为XML的轻量级的C++库。

## 10、C++名库——字符串

１、C++ Str Library 参考网站：http://www.utilitycode.com/str/
> 操作字符串和字符的库，支持Windows和支持gcc的多种平台。提供高度优化的代码，并且支持多线程环境和Unicode，同时还有正则表达式的支持。

２、Common Text Transformation Library 参考网站：http://cttl.sourceforge.net/
> 这是一个解析和修改STL字符串的库。CTTL substring类可以用来比较，插入，替换以及用EBNF的语法进行解析。

３、GRETA 参考网站：http://research.microsoft.com/projects/greta/
> 这是由微软研究院的研究人员开发的处理正则表达式的库。在小型匹配的情况下有非常优秀的表现。

## 11、C++名库——综合

１、P::Classes　参考网站：http://pclasses.com/
> 一个高度可移植的C++应用程序框架。当前关注类型和线程安全的signal/slot机制，i/o系统包括基于插件的网络协议透明的i/o架构，基于插件的应用程序消息日志框架，访问sql数据库的类等等。

２、ACDK - Artefaktur Component Development Kit 参考网站：http://acdk.sourceforge.net/
> 这是一个平台无关的C++组件框架，类似于Java或者.NET中的框架（反射机制，线程，Unicode，废料收集，I/O，网络，实用工具，XML，等等），以及对Java, Perl, Python, TCL, Lisp, COM 和 CORBA的集成。

３、dlib C++ library 参考网站：http://www.cis.ohio-state.edu/~kingd/dlib/
> 各种各样的类的一个综合。大整数，Socket，线程，GUI，容器类,以及浏览目录的API等等。

４、Chilkat C++ Libraries 参考网站：http://www.chilkatsoft.com/cpp_libraries.asp
> 这是提供zip，e-mail，编码，S/MIME，XML等方面的库。

５、C++ Portable Types Library (PTypes) 参考网站：http://www.melikyan.com/ptypes/
> 这是STL的比较简单的替代品，以及可移植的多线程和网络库。

６、LFC 参考网站：http://lfc.sourceforge.net/
> 哦，这又是一个尝试提供一切的C++库

## 12、C++名库——其他库

１、Loki 参考网站：http://www.moderncppdesign.com/
> 哦，你可能抱怨我早该和Boost一起介绍它，一个实验性质的库。作者在loki中把C++模板的功能发挥到了极致。并且尝试把类似设计模式这样思想层面的东西通过库来提供。同时还提供了智能指针这样比较实用的功能。

２、ATL
> ATL(Active Template Library)是一组小巧、高效、灵活的类，这些类为创建可互操作的COM组件提供了基本的设施。

３、FC++: The Functional C++ Library
> 这个库提供了一些函数式语言中才有的要素。属于用库来扩充语言的一个代表作。如果想要在OOP之外寻找另一分的乐趣，可以去看看函数式程序设计 的世界。大师Peter Norvig在 “Teach Yourself Programming in Ten Years”一文中就将函数式语言列为至少应当学习的6类编程语言之一。

４、FACT! 参考网站：http://www.kfa-juelich.de/zam/FACT/start/index.html
> 另外一个实现函数式语言特性的库

５、Crypto++
> 提供处理密码，消息验证，单向hash，公匙加密系统等功能的免费库。

**还有很多非常激动人心或者是极其实用的C++库，限于我们的水平以及文章的篇幅不能包括进来。在对于这些已经包含近来的库的介绍中，由于并不是每一个我们都使用过，所以难免有偏颇之处，请读者见谅。**

## 13、C++名人的网站

正如我们可以通过计算机历史上的重要人物了解计算机史的发展，C++相关人物的网站也可以使我们得到最有价值的参考与借鉴，下面的人物我们认为没有 介绍的必要，只因下面的人物在C++领域的地位众所周知，我们只将相关的资源进行罗列以供读者学习，他们有的工作于贝尔实验室，有的工作于知名编译器厂 商，有的在不断推进语言的标准化，有的为读者撰写了多部千古奇作……

１、Bjarne Stroustrup

http://www.research.att.com/~bs/

２、Stanley B. Lippman

http://blogs.msdn.com/slippman/ (中文版)

http://www.zengyihome.net/slippman/index.htm

３、Scott Meyers

http://www.aristeia.com/

４、David Musser

http://www.cs.rpi.edu/~musser/

５、Bruce Eckel

http://www.bruceeckel.com

http://blog.csdn.net/beckel Bruce Eckel 的中文版博客

６、Nicolai M. Josuttis

http://www.josuttis.com/

７、Herb Sutter

http://www.gotw.ca/

http://blog.csdn.net/hsutter/ Herb Sutter 中文博客

８、Andrei Alexandrescu

http://www.moderncppdesign.com

## 14、C++开源跨平台类库及在VC++.net中应用的配置

## 15、C++资源之不完全导引