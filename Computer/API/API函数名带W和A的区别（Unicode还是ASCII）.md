# API函数名带W和A的区别（Unicode还是ASCII）

 不知道各位新手朋友们遇到这样的问题没有呢，新建一个Windows应用程序，调用MessageBox这个函数，准备让它弹出一段提示文本，可是编译器在编译的时候却报错说，不能将 const char* 或者 const char[] 转换为 const wchar_t* 之类的提示呢，很多刚接触Windows API编程的朋友们在这里可能就卡住了，不知如何下手解决了，其实，这就是Unicode编码和ASCII编码的问题了。我下面就会一一道来

关于Unicode和ASCII具体的编码是怎么的，我这里就不详细介绍了，也介绍不了，如果需要深入了解，网上有很多这方面的专门文章，我这里就只对Unicode编码和ASCII编码在Windows平台下的编程相关的内容进行介绍。
我们都知道Unicode和ASCII最大的区别就是Unicode采用2个字节来存储一个字符，不管是英文，汉字，还是其他国家的文字，都有能用2个字节来进行编码，而ASCII采用一个字节存储一个字符，所以对于英文的编码，那是足够的了，可是对于汉字的编码，则必须采用一些特殊的方法，用2个ASCII字符来表示一个汉字。

我们在写程序的过程中，势必要和字符打交道，要输入，获取，显示字符，到底是选用Unicode字符呢还是ASCII字符呢，这都是各位自己的权利。但为了程序的通用性和符合目前操作系统的主流趋势，Unicode编码是被推荐的。由于Unicode字符要比ASCII字符占用的空间大一倍，编译出来的程序在体积上和占用的内存上必定要大一些，不过这并不是什么很大的问题。所以微软目前的SDK中保留了2套API，一套用于采用Unicode编码处理字符的程序的编写，一套用于采用ASCII编码处理字符的程序的编写。 例如，我们上面提到的MessageBox，它其实不是一个函数名，而是一个宏定义，我们先来看看它是怎么被定义的，再来讨论它。

#ifdef UNICODE
    #define  MessageBox  MessageBoxW
#else
    #define MessageBox  MessageBoxA
#endif

看到了吗?  很简单是不是, 如果定义了UNICODE 这个宏 那么就定义MessageBox为MessageBoxW，如果没有定义UNICODE这个宏， 那么就定义MessageBox 为MessageBoxA，MessageBox后面的W和A 就是代表宽字节(Unicode)和ASCII，这样，其实存在于SDK中的函数是MessageBoxW和MessageBoxA这两个函数.
MessageBox只是一个宏而已。所以在程序中，这3个名字你都可以使用，只不过需要注意的是，使用MessageBoxA的话，那么你要注意传给它的参数，字符都必须是单字节，也就是ASCII， 在程序中就是char，如果使用MessageBoxW的话，那么，字符都必须使用Unicode，程序中就是 wchar_t。 但是这样有个非常不方便的地方那就是，如果你使用W后缀系列的函数的话，那么你的程序使用的字符就是Unicode字符编码的，但是如果你需要用这个程序的源代码编译出字符采用ASCII编码的程序，那么需要改动的地方就太大了。凡是涉及到字符操作的地方都需要改变。那么 ，有没有比较好的办法不做更改就可以用同样的代码编译出ASCII版本的程序呢。  
当然有，就是我们在编程的时候尽量使用不带后缀的宏定义，如上例，就使用MessageBox,其中的参数也不明确使用char 还是wchar_t 而是使用微软给我们定义的TCHAR字符数据类型，它的定义和上面MessageBox函数的定义差不多，都是根据是否定义了UNICODE这个宏来判断是将TCHAR定义为char还是wchar_t，所以这样一来，这个TCHAR的数据类型就是可变的了，它根据工程的设置而定义为相应的最终字符类型，这样我们的程序就可以不做任何更改就可以轻松的编译出另外一个版本的了。是不是非常方便。
前面2篇文章纯文字的介绍比较多，因为很多是概念性的，需要理解，后面的文章我准备配合一些小示例程序，使用一些简单的API函数，遇到的相关的概念在一并介绍的方法进行。所以，前2篇文章如果各位朋友不是很能理解，不用担心，影响不是很大，经过后面的学习，你就会慢慢的理解前面所说的内容了。