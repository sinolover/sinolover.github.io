 有小伙伴问我每天忽悠的TPL是什么？
 
 这次站位高一点，严肃讲一讲。

# 引言

作为微软技术栈的老鸟，一直将代码整洁之道奉为经典， 优秀的程序员将优雅、高性能的代码看成自己的脸面。  

今天探讨下我对.NET并行编程库`Task Parallel Library`的理解，开足马力，准备压榨CPU了。

![](vx_images/330455515253598.gif "null") 
双核cpu的真相.gif

# 技术背景

## 硬件线程和软件线程

   多核处理器带有一个以上的物理内核：物理内核是真正的独立处理单元，多个物理内核使得多条指令能够同时并行运行。

**硬件线程也称为逻辑内核**，一个物理内核可能会使用超线程技术提供多个硬件线程，所以一个硬件线程并不代表一个物理内核。程序通过`Environment.ProcessorCount` 得到的就是逻辑内核（本人的机器是i5-5300U 虚拟4核）， Windows中每个运行的程序都是一个进程，每一个进程都会创建并运行一个或多个线程，这些线程称为软件线程，硬件线程就像是一条泳道，而软件线程就是在其中游泳的人。

## 并行场景

.NET引入的Task Parallel Library(任务并行库，TPL)，动态地扩展并发度，以最有效的方式使用所有可用的处理器。

另外TPL支持分区工作、支持基于ThreadPool调度、支持取消异步操作、支持状态管理。

通过TPL专注于 让程序完成业务意义上的任务，同时最大限度的提高程序性能。

TPL同时支持数据并行、任务并行和流水线Dataflow

1.**数据并行**：有大量数据需要处理，并且必须对每一份数据执行同样的操作；2.**任务并行**：通过任务并发运行不同的操作；3.**流水线**：任务并行和数据并行的结合体（需要引入System.Threading.Tasks.Dataflow组件库）   
其中1、3 已经在[上文](http://mp.weixin.qq.com/s?__biz=MzI4NTU0NjYwOA%3D%3D&chksm=ebebcca2dc9c45b4fbf93ffbcc95efb076b1c1032e1935c378d77b06c25c34455b6d7d830a48&idx=1&mid=2247486909&scene=21&sn=c32cf4c36130c428fc79aa5546c2bc06#wechat_redirect)演示，本文就随手拿数据并行、任务并行聊一聊。

# 编程实践

## 1\. 数据并行

> 找到100000以内素数的个数

上文\[共享内存并发模型\],代码可做如下优化：

由每个线程独立计算线程内迭代产生的素数和，最后再对几个和求和。

```go
using System;
using System.Threading.Tasks;
using System.Collections;
using System.Collections.Generic;
using System.Threading;
using System.Diagnostics;

/// <summary>
/// 利用并行编程库Parallel，计算100000内素数的个数
/// </summary>
namespace Paralleler
{
    class Program
    {
        static void Main(string[] args)
        {
            Stopwatch sw = new Stopwatch();

            sw.Start();
            ShareMemory();
            sw.Stop();
            Console.WriteLine($"优化后的共享内存并发模型耗时：{sw.Elapsed}");
        }

        static void ShareMemory()
        {
            var sum = 0;
            Parallel.For(1, 100000 + 1, () => 0, (x, state, local) =>
            {
                var f = true;
                if (x == 1)
                    f = false;
                for (int i = 2; i <= x / 2; i++)
                {
                    if (x % i == 0)  // 被[2,x/2]任一数字整除，就不是质数
                        f = false;
                }
                if (f == true)
                    local++;
                return local;
            },
                 local =>
                 {
                     Interlocked.Add(ref sum, local);
                 }
               );
            Console.WriteLine($"1-100000内质数的个数是{sum}");
        }
    }
}
```

```
参数1,2 表示数据并行要操作的对象；
参数3localInit表示某线程内迭代的初始值，将会作为参数4body委托的第3个参数，只在线程第一次使用;

参数4body表示每个迭代都需要经历的执行体, 这里以线程为单元处理迭代；
参数5localFinally对每个线程的输出再做一次计算，入参是参数4的输出。
```

## 2\. 任务并行

  让许多方法并行运行的最简单的方法就是使用Parallel类的`Invoke`方法，`Invoke方法接受一个Action的参数组`

```
void  System.Threading.Tasks.Parallel.Invoke(WatchMovie, HaveDinner, ReadBook, WriteBlog);
```

这段代码会创建指向每一个方法的委托。

> **没有特定的执行顺序**
> 
> Parallel.Invoke方法只有在4个方法全部完成之后才会返回。它至少需要4个硬件线程才足以让这4个方法并发运行。
> 
>   
> 但并不保证这4个方法能够同时启动运行，如果一个或者多个内核处于繁忙状态，那么底层的调度逻辑可能会延迟某些方法的初始化执行。

## 捕捉并行循环中发生的异常

当并行迭代中调用的委托抛出异常，这个异常没有在委托中被捕获到时，就会变成一组异常，新的`System.AggregateException`负责处理这一组异常。

![](vx_images/328115515258462.gif)

本文为微软TPL入门级教程，学习一个专题，了解特性/能力最重要， 剩下的就是结合场景去应用。

本文内容和制图均为原创，文章永久更新地址请参阅左下角原文，如对您有所帮助，【在看、点赞】来一发，未尝不可 。

**专题相关 一网打尽**

![](vx_images/327045515236022.gif)

  [三分钟掌握共享内存 & Actor并发模型](http://mp.weixin.qq.com/s?__biz=MzI4NTU0NjYwOA%3D%3D&chksm=ebebcca2dc9c45b4fbf93ffbcc95efb076b1c1032e1935c378d77b06c25c34455b6d7d830a48&idx=1&mid=2247486909&scene=21&sn=c32cf4c36130c428fc79aa5546c2bc06#wechat_redirect)\~~~

![](vx_images/325975515254781.gif)

 [你管这叫"线程安全"?](http://mp.weixin.qq.com/s?__biz=MzI4NTU0NjYwOA%3D%3D&chksm=ebebc99fdc9c4089520b40728bc831e09c73d14a3e96559eaeeaaeff4e8341b3662dad45b2b7&idx=1&mid=2247485568&scene=21&sn=7a2e5fc55781d1462133061bef843047#wechat_redirect)\~~~

![](vx_images/324625515257279.gif)

 [.Net线程同步技术解读](http://mp.weixin.qq.com/s?__biz=MzI4NTU0NjYwOA%3D%3D&chksm=ebebc05ddc9c494b893c7ffccd094f349557fcb47cb50c02d99975eea759a7ef19c968a3f346&idx=1&mid=2247483714&scene=21&sn=c95c7f69e19eb29aa4aefa3d915e6df6#wechat_redirect)

![](vx_images/323535515259675.gif) [Redis分布式锁抽丝剥茧](http://mp.weixin.qq.com/s?__biz=MzI4NTU0NjYwOA%3D%3D&chksm=ebebc8d5dc9c41c3f3cca7edc8b1f6642d7ba0714e4209f6e25d351692204b1554a15ff3e7b4&idx=1&mid=2247485898&scene=21&sn=cfef01bd375043f198ce5e42653d6c16#wechat_redirect)

![](vx_images/322185515241795.gif)

 [看过这么多爆文，依旧走不好异步编程这条路？](http://mp.weixin.qq.com/s?__biz=MzI4NTU0NjYwOA%3D%3D&chksm=ebebc86ddc9c417bfa85e6468bdfb7f8ae0c93cddfa85214ebbcbb56097c541b9ce0a647178b&idx=1&mid=2247485810&scene=21&sn=49e5e11b1be01c25902fffa3cf7803e3#wechat_redirect)  

![](vx_images/321115515246041.gif)

 [TPL Dataflow组件应对高并发,低延迟要求](http://mp.weixin.qq.com/s?__biz=MzI4NTU0NjYwOA%3D%3D&chksm=ebebc060dc9c4976e4d125d7f2a756d51e5d409006d5a0a70a3b3bd1f0ecdb40561d01dfa6ad&idx=2&mid=2247483775&scene=21&sn=8f1d422a940b9782d75b4e58f438c898#wechat_redirect)

![](vx_images/319815515249486.gif)

 [如何利用.NETCore向Azure EventHubs准实时批量发送数据？](http://mp.weixin.qq.com/s?__biz=MzI4NTU0NjYwOA%3D%3D&chksm=ebebc525dc9c4c336fe2175736b307352141259949ad138e39de08a3dc62d0469cce589b598e&idx=1&mid=2247484474&scene=21&sn=0b73d7a5cf933740ec3f6361edb65f6a#wechat_redirect)  

![](vx_images/318445515256817.gif)

 [难缠的布隆过滤器，这次终于通透了](http://mp.weixin.qq.com/s?__biz=MzI4NTU0NjYwOA%3D%3D&chksm=ebebca7adc9c436c379b1ff2cc0efa8b2e8d2295fe39eafa8fd3b300968dbf2c41219a91682b&idx=1&mid=2247486309&scene=21&sn=74f466db12ce1283fdc6a9b548ea8a10#wechat_redirect)

扫码关注我们

不会让您失望的。