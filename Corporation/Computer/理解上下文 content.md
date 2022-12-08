# 理解上下文 content
java 中的 context上下文，其实 翻译成“环境”、“容器”可能更好
**所谓的上下文是因为在Java的三大框架下编程的时候，是侵入式的，所以很多变量、对象已经被框架提前定义好了，只有全面了解在不同的*环境* *容器* 里所能使用的变量、对象，才能更好的编程** bySteven
Context在Java中的出现是如此频繁，但其中文翻译“上下文”又是如此诡异拗口，因此导致很多人不是很了解Context的具体含义是指什么，所以很有必要来深究一下这词的含义。先来举几个JAVA中用到Context的例子（1）JNDI的一个类javax.naming.InitialContext，它读取JNDI的一些配置信息，并内含对象和其在JNDI中的注册名称的映射信息。请看下面的代码

InitialContext ic=new InitialContext();

RMIAdaptor server=(RMIAdaptor)ic.lookup("jmx/invoker/RMIAdaptor");

这是一段JBoss中获取MBean的远程调用类的代码。在这里面通过InitialContext中JNDI注册的名称“jmx/invoker/RMIAdaptor”来获得RMIAdaptor对象。
这和JAVA集合中的MAP有点象，有一个String的key，key对映着它的对象。（2）再来看看下面Spring中最常见的几句代码。ApplicationContext是内含configuration.xml配置文件的信息，使得可以通过getBean用名称得到相应的注册对象。

ApplicationContext ctx= newFileSystemXmlApplicationContext("configuration.xml");

Object obj= ctx.getBean("Object_Name");

从上面的代码，我很能体会到**Context****所代表的意义：公用信息、环境、容器**....。所以我觉得Context翻译成上下文并不直观，按照语言使用的环境，翻译成“环境”、“容器”可能更好。把Context翻译成“上下文”只是不直观罢了，不过也没大错。我们来看看中文的“上下文”是什么意思。我们常说听话传话不能“断章取义”，而要联系它的“上下文”来看。比如，小丽对王老五说“我爱你”，光看这句还以为在说情话呢。但一看上下文－－“虽然我爱你，但你太穷了，我们还是分手吧”，味道就完全变了。从这里来看“上下文”也有“环境”的意思，就是语言的环境。

 PS：

 上下文其实是一个抽象的概念。我们常见的上下文有Servlet中的pageContext，访问JNDI时候用的Context。写过这些代码的人可能比较容易理解，其实他们真正的作用就是承上启下。比如说pageContext他的上层是WEB容器，下层是你写的那个Servlet类，pageContext作为中间的通道让Servlet 和Web容器进行交互。再比如访问JNDI的Context，他的上层是JNDI服务器（可能是远程的），下层是你的应用程序，他的作用也是建立一个通道让你能访问JNDI服务器，同时也让JNDI服务器接受你的请求，同样起到交互作用。