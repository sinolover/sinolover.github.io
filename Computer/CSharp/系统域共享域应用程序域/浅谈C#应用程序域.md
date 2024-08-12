# 浅谈C#应用程序域
在以前传统的开发中我们都知道，一个应用程序对应一个进程，并为该进程指定虚拟内存，由操作系统来映射实际的物理内存，有效的维护了进程之间的安全性。但另一方面，每一个进程都会消耗一定的系统资源，降低了性能，并且进程间的通信也比较麻烦。  
在.NET中推出了一个新的概念：C#应用程序域(AppDomain)。可以理解成很多应用程序域都可以运行在同一个.NET的进程中，可以降低系统消耗，同时不同的域之间互相隔离，在安全性方面有保障。另外对于同一个进程内不同域之间的通信也相对简单一点。  
应用程序域涉及的内容很多，本文就简要描述以下两个方面：  
1、如何创建、卸载域  
2、如何实现域间的通信  
一、如何创建、卸载域  
在.NET中提供了AppDomain类为执行托管代码提供隔离、卸载和安全边界。
~~~csharp

AppDomainSetup info = new AppDomainSetup();  
info.LoaderOptimization = LoaderOptimization.SingleDomain;  
AppDomain domain = AppDomain.CreateDomain("MyDomain", null, info);  
domain.ExecuteAssembly("C:\test\DomainCom.exe");  
AppDomain.Unload(domain); 
~~~

1、使用AppDomainSetup类定义新域的属性，比如可以设置应用程序的根目录，设置被加载程序的类别。  
例子中使用的是SingleDomain表示加载程序不得在C#应用程序域之间共享内部资源，还可以使用MultiDomain、MultiDomainHost等其他属性  
2、在第四行创建一个名字为MyDomain的新域  
3、在第5行在新域内部执行一个应用程序  
4、第6行卸载这个新域  
通过这样创建后，新域的执行就算出现系统异常也不会影响到原来域的执行，那么就可以做类似WatchDog(监控子程序，一旦退出就重启)的程序了  
二、如何实现域间的通信  
公共语言运行库禁止在不同域中的对象之间进行直接调用，但我们可以复制这些对象，或通过代理访问这些对象
~~~csharp

1. AppDomain Setupinfo2=new AppDomainSetup();  
2. info2.LoaderOptimization=LoaderOptimization.SingleDomain;  
3. info2.ApplicationBase="C:\test";  
4. AppDomain AppDomaindomain2=AppDomain.CreateDomain("MyDomain2",null,info2);  
5. ObjectHandle objHandle=domain2.CreateInstance("DomainCom","DomainCom.TestStatic");  
6. ICollection obj=objHandle.Unwrap() as ICollection;  
7. int i=obj.Count;  
8. domain2.ExecuteAssembly("C:\test\DomainCom.exe");  
9. AppDomain.Unload(domain2); 
~~~

开始的代码都差不多，重点是以下几个方面：  
1、第5行在新域中创建一个对象(类DomainCom.TestStatic)，并返回一个代理ObjectHandle 类 用于在多个C#应用程序域之间传递对象  
     DomainCom.TestStatic必须从MarshalByRefObject类继承，为了演示方便，这个类很简单，从ICollection接口继承，就实现了一个Count属性：
~~~csharp

usingSystem;  
usingSystem.Collections.Generic;  
usingSystem.Text;  
usingSystem.Collections;  
  
namespace DomainCom  
{  
    public class TestStatic:MarshalByRefObject,ICollection  
    {  
        private static int count=1;  
        
        public int Count  
        get 
        {  
            count=count*2;  
            return count;  
        }
    }  
}  
#region未实现代码
public bool IsSynchronized{get{throw new NotImplementedException();}}  
  
public object SyncRoot{get{throw new NotImplementedException();}}  
  
public void CopyTo(Array array,intindex)  
{  
    thrownewNotImplementedException();  
}  
public IEnumerator GetEnumerator()  
{  
    throw new NotImplementedException();  
}  
#endregion

~~~
2、第6行取得新域中的对象  
3、第7行在当前域中给新域中的对象赋值  
4、第8行执行新域中的应用程序，这个应用程序中就是弹出一个对话框显示Count的值。
~~~cs
TestStatic test = new TestStatic();
MessageBox.Show(test.Count.ToString());
~~~

得到的结果为 4， 证明实现了域间对象的互操作，这样我们就可以实现其他更复杂的操作了。