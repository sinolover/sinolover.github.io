# (二)-volatile、Interlocked、ReaderWriterLockSlim
上章主要讲排他锁的直接使用方式。但实际当中全部都用锁又太浪费了，或者排他锁粒度太大了，本篇主要介绍下升级锁和原子操作。

## volatile

简单来说volatile关键字是告诉c#编译器和JIT编译器，不对volatile标记的字段做任何的缓存。确保字段读写都是原子操作，最新值。

从功能上看起到锁的作用，但它不是锁， 它的原子操作是基于CPU本身的，非阻塞的。 
（因为32位CPU执行赋值指令，数据传输最大宽度4个字节，所以只要在4个字节以下读写操作的，32位CPU都是原子操作，volatile 是利用这个特性来保证其原子操作的。)

这样的目的是为了提高JIT性能效率，对有些数据进行缓存了(多线程下)。



```cs
//正确
public volatile Int32 score1 = 1;
//报错
public volatile Int64 score2 = 1;
```

如上，我们尝试定义了8个字节长度score2，会抛出异常。  因为8个字节32位CPU就分成2个指令执行了，所以就无法保证其原子操作了。

如果把编译平台改成64位，同样不可以使用，C#限制4个字节以下的类型字段才能用volatile。

![](vx_images/467222514268952.png)

还一种方法是使用特定平台的整数类型IntPtr，这样volatile即可作用到64位上了。

volatile多数情况下很有用处的，毕竟锁的性能开销还是很大的。可以把volatile当成轻量级的锁，根据具体场景合理使用，能提高不少程序性能。

线程中的Thread.VolatileRead 和Thread.VolatileWrite 是volatile以前的版本。

## Interlocked

MSDN 描述：为多个线程共享的变量提供原子操作。主要函数如下：

Interlocked.Increment　　　　原子操作，递增指定变量的值并存储结果。 
Interlocked.Decrement　　     原子操作，递减指定变量的值并存储结果。 
Interlocked.Add　　　　　　   原子操作，添加两个整数并用两者的和替换第一个整数
Interlocked.CompareExchange(ref a, b, c);  原子操作，a参数和c参数比较，  相等b替换a，不相等不替换。

下面是个interlock anything的例子：


```cs
public static int Maximum(ref int target, int value)
{
    int currentVal = target, startVal, desiredVal;  //记录前后值
    do
    {
        startVal = currentVal; //记录循环迭代的初始值。
        desiredVal = Math.Max(startVal, value); //基于startVal和value计算期望值desiredVal

        //高并发下，线程被抢占情况下，target值会发生改变。

        //target startVal相等说明没改变。desiredVal 直接替换。
        currentVal = Interlocked.CompareExchange(ref target, desiredVal, startVal);

    } while (startVal != currentVal); //不相等说明，target值已经被其他线程改动。自旋继续。
    return desiredVal;
}
```

## ReaderWriterLockSlim

假如有份缓存数据A，如果每次都不管任何操作lock一下，那么我的这份缓存A就永远只能单线程读写了， 这在Web高并发下是不能忍受的。

那有没有一种办法我只在写入时进入独占锁呢，读操作时不限制线程数量呢？答案就是我们的ReaderWriterLockSlim主角，读写锁。

ReaderWriterLockSlim 其中一种锁EnterUpgradeableReadLock最关键  即可升级锁。  

它允许你先进入读锁，发现缓存A不一样了， 再进入写锁，写入后退回读锁模式。

ps： 这里注意下net 3.5之前有个ReaderWriterLock 性能较差。推荐使用升级版的 ReaderWriterLockSlim 。 

```cs
//实例一个读写锁
 ReaderWriterLockSlim cacheLock = new ReaderWriterLockSlim(LockRecursionPolicy.SupportsRecursion);
```

上面实例一个读写锁，这里注意的是构造函数的枚举。

LockRecursionPolicy.NoRecursion 不支持，发现递归会抛异常。

LockRecursionPolicy.SupportsRecursion  即支持递归模式，线程锁中继续在使用锁。


```cs
cacheLock.EnterReadLock();
//do 
cacheLock.EnterReadLock();
//do
cacheLock.ExitReadLock();
cacheLock.ExitReadLock();
```

这种模式极易容易死锁，比如读锁里面使用写锁。


```cs
cacheLock.EnterReadLock();
//do 
cacheLock.EnterWriteLock();
//do
cacheLock.ExitWriteLock();
cacheLock.ExitReadLock();
```

下面是msdn的缓存例子了，加了注释。

```cs
public class SynchronizedCache
{
    private ReaderWriterLockSlim cacheLock = new ReaderWriterLockSlim();
    private Dictionary<int, string> innerCache = new Dictionary<int, string>();

    public string Read(int key)
    {
        //进入读锁，允许其他所有的读线程，写入线程被阻塞。
        cacheLock.EnterReadLock();
        try
        {
            return innerCache[key];
        }
        finally
        {
            cacheLock.ExitReadLock();
        }
    }

    public void Add(int key, string value)
    {
        //进入写锁，其他所有访问操作的线程都被阻塞。即写独占锁。
        cacheLock.EnterWriteLock();
        try
        {
            innerCache.Add(key, value);
        }
        finally
        {
            cacheLock.ExitWriteLock();
        }
    }

    public bool AddWithTimeout(int key, string value, int timeout)
    {
        //超时设置，如果在超时时间内，其他写锁还不释放，就放弃操作。
        if (cacheLock.TryEnterWriteLock(timeout))
        {
            try
            {
                innerCache.Add(key, value);
            }
            finally
            {
                cacheLock.ExitWriteLock();
            }
            return true;
        }
        else
        {
            return false;
        }
    }

    public AddOrUpdateStatus AddOrUpdate(int key, string value)
    {
        //进入升级锁。 同时只能有一个可升级锁线程。写锁，升级锁都被阻塞，但允许其他读取数据的线程。
        cacheLock.EnterUpgradeableReadLock();
        try
        {
            string result = null;
            if (innerCache.TryGetValue(key, out result))
            {
                if (result == value)
                {
                    return AddOrUpdateStatus.Unchanged;
                }
                else
                {
                    //升级成写锁，其他所有线程都被阻塞。
                    cacheLock.EnterWriteLock();
                    try
                    {
                        innerCache[key] = value;
                    }
                    finally
                    {
                        //退出写锁，允许其他读线程。
                        cacheLock.ExitWriteLock();
                    }
                    return AddOrUpdateStatus.Updated;
                }
            }
            else
            {
                cacheLock.EnterWriteLock();
                try
                {
                    innerCache.Add(key, value);
                }
                finally
                {
                    cacheLock.ExitWriteLock();
                }
                return AddOrUpdateStatus.Added;
            }
        }
        finally
        {
            //退出升级锁。
            cacheLock.ExitUpgradeableReadLock();
        }
    }

    public enum AddOrUpdateStatus
    {
        Added,
        Updated,
        Unchanged
    };
}
```

多线程实际开发当中一直是个难点，特别是并发量比较高的情况下，使用锁时尤为注意。