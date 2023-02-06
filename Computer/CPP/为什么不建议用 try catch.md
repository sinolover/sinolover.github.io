作者：cabbage
链接：https://www.zhihu.com/question/29459586/answer/85859852
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

1. C++，是不推荐用try catch的，它推荐使用Windows API那种HResult来返回错误情况，原因是try catch会在已有的代码上面增加额外的cost, 这个额外的cost不是说只有throw exception的时候才会有，而是在try catch block里面的每一行代码中都会有，这也是为什么他不建议你使用try catch最主要的原因。在Windows的源代码中，是没有任何try catch的，全部用HResult来处理。
2. C#, try catch是建议使用的，C#设计的时候吸取的C++ try catch的教训，所以直接用Try catch包裹已有代码增加的cost可以忽略不计，但是如果真的在代码运行过程中throw exception了，这个cost还是很大的。所以，在C#代码设计中，throw exception基本上是你认为不会发生这种意外的情况下，否则，如果是常见错误，最好不要throw exception。
3. Java, try catch也是建议使用的，我这个用的不熟，不过看它的说明，即使是throw exception的时候的cost也很小。

总结说来，try catch是否建议使用要看具体语言，最重要衡量的标准就是它对已有的代码性能有多大的影响。但是从它设计的角度就是为了处理一些意料不到的情况，但是因为当初引入的时候各种各样的原因，导致有些语言为了性能，不推荐使用。BTW,  try catch最好不要catch (Exception), 这样会吃掉不该吃的问题，比如C#中的StackOverflowException, OutOfMemoryException, NullReferenceException etc. 该crash的时候就应该让App crash, restart, 这也是保护你service的一个好方法。





作者：知乎用户v46phN  
链接：https://www.zhihu.com/question/29459586/answer/44494726  
来源：知乎  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。  
  

不问是不是，就问为什么。这个问题看来需要从头说起。

一句话解释：

[try catch机制](https://www.zhihu.com/search?q=try%20catch%E6%9C%BA%E5%88%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A44494726%7D)非常好。那些觉得try catch不行的人，是他们自己的水平有问题，无法理解这种机制。并且这群人写代码不遵守规则，喜欢偷懒，这才造成try catch不好的错觉。

详细解释：

1.程序要健壮，必须要设计报错机制。

最古老，也是最常见的，比如：
~~~
bool CreateFile( );
//如果创建文件失败就返回false，否则返回true。
~~~

这种报错方式，显然不好。因为它没有给出产生错误的具体原因。

2.改进：一个函数或过程，会因为不同的原因产生错误，报错机制必须要把这些错误原因进行区分后，再汇报。
比如：
~~~
int CreateFile():

//如果创建成功就返回1.

//如果是因为没有权限，导致失败，返回-1。

//如果是因为文件已经存在，导致失败，返回-2。

//如果是因为创建文件发生超时，导致失败，返回-3。
~~~

这样看上去，比【1】要好些，至少指出了比较具体的失败原因，但是，还不够。

3.很多情况下，函数需要把详细的原因，用字符串的方式，返回：
~~~
class Result
{
....int State;//同【2】
....string ErrorMessage;//如果失败，这里将给出详细的信息，如果有可能，应该把建议也写上去。
}

Result CreateFile();

//如果创建成功，返回的Result，State为1，ErrorMessage为null。

//如果是因为没有权限，导致失败，返回的Result，State为-1，ErrorMessage为"用户【guest】没有权限在【C:】这个目录下创建该文件。建议您向管理员申请权限，或者更换具有权限的用户。"。

//如果是因为文件已经存在，导致失败，返回的Result，State为-2，ErrorMessage为"文件【C:abc.txt】已经存在。如果需要覆盖，请添加参数：arg_overwrite = true"。

//如果是因为创建文件发生超时，导致失败，返回的Result，State为-3，ErrorMessage为"在创建文件时超时，请使用chkdsk检查文件系统是否存在问题。"。
~~~

4.我个人推崇上面这种方式，完整，美观。但是这种流程，容易与正常的代码混在一起，不好区分开。因此，Java、C#等设计了try catch这一种特殊的方式：
~~~
void CreateFile()

//如果创建成功就不会抛出异常。

//如果是因为没有权限，导致失败，会抛出AccessException，这个Exception的Msg属性为"用户【guest】没有权限在【C:】这个目录下创建该文件。建议您向管理员申请权限，或者更换具有权限的用户。"。

//如果是因为文件已经存在，导致失败，会抛出FileExistedException，这个Exception的Msg属性为"文件【C:abc.txt】已经存在。如果需要覆盖，请添加参数：arg_overwrite = true"。

//如果是因为创建文件发生超时，导致失败，会抛出TimeoutException，这个Exception的Msg属性为"在创建文件时超时，请使用chkdsk检查文件系统是否存在问题。"。
~~~
可见，上述机制，实际上是用不同的Exception代替了【3】的State。

这种机制，在外层使用时：
~~~
try
{
....CreateFile( "C:abc.txt" );
}
catch( AccessException e )
{
....//代码进入这里说明发生【没有权限错误】
}
catch( FileExistedException e )
{
....//代码进入这里说明发生【文件已经存在错误】
}
catch( TimeoutException e )
{
....//代码进入这里说明发生【超时错误】
}
~~~
对比一下【3】，其实这与【3】本质相同，只是写法不同而已。

5.综上，我个人喜欢【3】这类面向过程的写法。但很多喜欢面向对象的朋友，估计更喜欢【4】的写法。然而【3】与【4】都一样。这两种机制都是优秀的错误处理机制。

6.理论说完了，回到正题，题注问：为什么不用try catch？

答：这是因为，很多菜鸟，以及新手，他们是这样写代码的：
~~~
void CreateFile( )
//无论遇到什么错误，就抛一个 Exception，并且也不给出Msg信息。
~~~
这样的话，在外层只能使用：
~~~
try

{

....CreateFile( "C:abc.txt" );

}

catch( Exception e )

{

....//代码进入这里说明发生错误

}
~~~
当出错后，只知道它出错了，并不知道是什么原因导致错误。这同【1】。

以及，即使CreateFile是按【4】的规则设计的，但菜鸟在外层是这样使用的：
~~~
try

{

....CreateFile( "C:abc.txt" );

}

catch( Exception e )

{

....//代码进入这里说明发生错误

....throw Exception( "发生错误" )

}
~~~
这种情况下，如果这位菜鸟的同事，调用了这段代码，或者用户看到这个错误信息，也只能知道发生了错误，但并不清楚错误的原因。这与【1】是相同的。

出于这些原因，菜鸟的同事，以及用户，并没有想到，造成这个问题是原因菜鸟的水平太差，写代码图简单省事。他们却以为是try catch机制不行。

因此，这就导致了二逼同事，以及傻比用户，不建议用try catch。