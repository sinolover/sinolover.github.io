# 异步编程模式

.NET 提供了执行异步操作的三种模式[1](https://blog.csdn.net/FliesOfTime/article/details/100282584#fn1)：

*   APM：异步编程模型（Asynchronous Programming Model），这是使用 IAsyncResult 接口提供异步行为的旧模型。 在这种模式下，同步操作需要 Begin 和 End 方法（例如，BeginWrite 和 EndWrite以实现异步写入操作）。 不建议新的开发使用此模式。
*   EAP：基于事件的异步编程模式（Event-based Asynchronous Pattern），是提供基于事件的异步行为的旧模型。 这种模式需要后缀为 Async 的方法，以及一个或多个事件、事件处理程序委托类型和 EventArg 派生类型。 EAP 是在 .NET Framework 2.0 中引入的。 建议新开发中不再使用这种模式。
*   TAP：基于任务的异步编程模式（Task-based Asynchronous Pattern） ，该模式使用单一方法表示异步操作的开始和完成。 **TAP 是在 .NET Framework 4 中引入的，是在 .NET 中进行异步编程的推荐方法**。 C# 中的 async 和 await 关键词以及 Visual Basic 中的 Async 和 Await 运算符为 TAP 添加了语言支持。

# APM异步编程模型

## APM的本质

APM异步编写模型是一种模式，该模式允许用更少的线程去做更多的操作，.NET Framework很多类也实现了该模式，同时我们也可以自定义类来实现该模式，（也就是在自定义的类中实现返回类型为IAsyncResult接口的BeginXXX方法和EndXXX方法），另外委托类型也定义了BeginInvoke和EndInvoke方法[2](https://blog.csdn.net/FliesOfTime/article/details/100282584#fn2)。  
异步编程模型这个模式，是微软利用委托和线程池帮助我们实现的一个模式(该模式利用一个线程池线程去执行一个操作，在FileStream类BeginRead方法中就是执行一个读取文件操作，该线程池线程会立即将控制权返回给调用线程，此时线程池线程在后台进行这个异步操作；异步操作完成之后，通过回调函数来获取异步操作返回的结果。此时就是利用委托的机制。所以说异步编程模式时利用委托和线程池线程搞出来的模式，包括后面的基于事件的异步编程和基于任务的异步编程，还有C# 5中的async和await关键字，都是利用这委托和线程池搞出来的。他们的本质都一样，后面方法使异步编程更加简单。)

## APM的实现

下面就具体就拿FileStream类的BeginRead和EndRead方法来介绍下下异步编程模型的实现。

### 读取的同步方法：

```csharp
public override int Read(byte[] array, int offset, int count )；
```

读取过程中，执行此方法会阻塞主线程，最明显的就是造成界面卡死。

### BeginXxx方法——读取的异步方法：

```csharp
// 开始异步读操作
// 前面的3个参数和同步方法代表的意思一样，这里就不说了，可以看到这里多出了2个参数
// userCallback代表当异步IO操作完成时，你希望由一个线程池线程执行的方法,该方法必须匹配AsyncCallback委托
// stateObject代表你希望转发给回调方法的一个对象的引用，在回调方法中，可以查询IAsyncResult接口的AsyncState属性来访问该对象
public override IAsyncResult BeginRead(byte[] array, int offset, int numBytes, AsyncCallback userCallback, Object stateObject)
```

### EndXxx方法——结束异步操作

前面介绍完了BeginXxx方法，我们看到所有BeginXxx方法返回的都是实现了IAsyncResult接口的一个对象，并不是对应的同步方法所要得到的结果的。此时我们需要调用对应的EndXxx方法来结束异步操作，并向该方法传递IAsyncResult对象，EndXxx方法的返回类型就是和同步方法一样的。例如，FileStream的EndRead方法返回一个Int32来代表从文件流中实际读取的字节数。

### 异步调用的结果——与IAsyncResult对象的使用有关

对于访问异步操作的结果，APM提供了四种方式供开发人员选择：

*   在调用BeginXxx方法的线程上调用EndXxx方法来得到异步操作的结果，但是这种方式会阻塞调用线程，直到操作完成后，调用线程才继续运行。
*   查询IAsyncResult的AsyncWaitHandle属性，从而得到WaitHandle，然后再调用它的WaitOne方法来使一个线程阻塞并等待操作完成再调用EndXxx方法来获得操作的结果
*   循环查询IAsyncResult的IsComplete属性，操作完成后再调用EndXxx方法来获得操作返回的结果。
*   使用 AsyncCallback委托来指定操作完成时要**调用的方法**，并在操**调用的方法**中调用EndXxx操作来获得异步操作的结果。**（最为常用）**

**个人感觉前三个方法都是一样的，没有本质区别。**

## APM异步编程伪代码

这段伪代码是按自己的理解写的，有点混乱：

```
        XXX()、BeginXXX()、EndXXX()方法都是官方类库定义好的方法，不涉及自己定义内容
        XXXCallBack()是自己定义的异步回调方法
        state对象是异步回调方法XXXCallBack要使用的对象，可以为null
        state对象我一般是取BeginXXX方法所处的类实例，以便在XXXCallBack()中调用EndXXX()方法（推荐）
        可通过BeginXXX()返回的IAsyncResult对象的IsCompleted属性判断异步操作是否完成（不推荐使用）


        同步方法：
        返回参数类型 XXX(参数1，参数2)

        异步Begin方法：
        IAsyncResult BeginXXX（参数1，参数2，new AsyncCallback(XXXCallBack),state对象）

        异步CallBack方法(内部调用EndXXX())：
        void XXXCallBack(IAsyncResult result)
        {
            强制还原result.AsyncState为state对象
            state对象类型 state对象 = (state对象类型)result.AsyncState;

            结束异步回调并取回返回参数
            返回参数类型 返回参数 = state对象.EndConnect(result);
        }
```

# EAP异步编程模式

当我们调用基于事件的异步模式的类的 XxxAsync方法时，即代表开始了一个异步操作，该方法调用完之后会使一个线程池线程去执行耗时的操作，所以当UI线程调用该方法时，当然也就不会堵塞UI线程了。并且基于事件的异步模式是建立了APM的基础之上的(这也是我在上一专题中详解介绍APM的原因)，而APM又是建立了在委托之上的（对于这点可以参考该系列的APM专题）。然而这点并不是凭空想象的，下面就BackgroundWorker类来给大家详解解释EAP是建立在APM的基础上的。

## 基于事件的异步模式示例

SoundPlayer 和 PictureBox 组件表示基于事件的异步模式的简单实现。 WebClient 和 **BackgroundWorker** 组件表示基于事件的异步模式的更复杂实现，BackgroundWorker的使用参考我的[C#中BackgroundWorker的使用](https://blog.csdn.net/FliesOfTime/article/details/100522211)

# TAP异步编程模式

## 使用 Async 和 Await 的异步编程

重点：

*   **方法签名包含 async 修饰符**，按照约定，异步方法的名称以“Async”后缀结尾
*   **方法通常包含至少一个 await 表达式**，该表达式标记一个点，在该点上，直到**等待的异步操作完成方法才能继续**。 同时，将方法挂起，并且**控制权返回到方法的调用方**。
*   异步方法在 await 表达式执行时暂停并不构成方法退出，只**会导致 finally 代码块不运行**
*   标记的异步方法本身可以通过调用它的方法等待
*   异步方法通常包含 await 运算符的一个或多个实例，但**缺少 await 表达式**也不会导致生成编译器错误。 如果异步方法未使用 await 运算符标记暂停点，那么异步方法即使有 async 修饰符也会作为同步方法执行，编译器将为**此类方法发布一个警告**

### 代码示例

官网上的示例已经很明确了，直接贴上来：  
[官网示例：](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/await)

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

public class AwaitOperator
{
    public static async Task Main()
    {
        Task<int> downloading = DownloadDocsMainPageAsync();
        Console.WriteLine($"{nameof(Main)}: Launched downloading.");

        int bytesLoaded = await downloading;
        Console.WriteLine($"{nameof(Main)}: Downloaded {bytesLoaded} bytes.");
    }

    private static async Task<int> DownloadDocsMainPageAsync()
    {
        Console.WriteLine($"{nameof(DownloadDocsMainPageAsync)}: About to start downloading.");
        
        var client = new HttpClient();
        byte[] content = await client.GetByteArrayAsync("https://docs.microsoft.com/en-us/");
        
        Console.WriteLine($"{nameof(DownloadDocsMainPageAsync)}: Finished downloading.");
        return content.Length;
    }
}
// Output similar to:
// DownloadDocsMainPageAsync: About to start downloading.
// Main: Launched downloading.
// DownloadDocsMainPageAsync: Finished downloading.
// Main: Downloaded 27700 bytes.
```

### 返回类型和参数

异步方法通常返回 Task 或 Task。 在异步方法中，await 运算符应用于通过调用另一个异步方法返回的任务。  
如果方法包含指定 TResult 类型操作数的 return 语句，将 Task 指定为返回类型。  
如果方法不含任何 return 语句或包含不返回操作数的 return 语句，将 Task 用作返回类型。

### 高效等待任务

[高效等待任务：](https://docs.microsoft.com/zh-cn/dotnet/csharp/programming-guide/concepts/async/#await-tasks-efficiently)

```csharp
static async Task Main(string[] args)
{
    Coffee cup = PourCoffee();
    Console.WriteLine("coffee is ready");
    var eggsTask = FryEggsAsync(2);
    var baconTask = FryBaconAsync(3);
    var toastTask = MakeToastWithButterAndJamAsync(2);

    var allTasks = new List<Task>{eggsTask, baconTask, toastTask};
    while (allTasks.Any())
    {
        Task finished = await Task.WhenAny(allTasks);
        if (finished == eggsTask)
        {
            Console.WriteLine("eggs are ready");
        }
        else if (finished == baconTask)
        {
            Console.WriteLine("bacon is ready");
        }
        else if (finished == toastTask)
        {
            Console.WriteLine("toast is ready");
        }
        allTasks.Remove(finished);
    }
    Console.WriteLine("Breakfast is ready!");

    async Task<Toast> MakeToastWithButterAndJamAsync(int number)
    {
        var toast = await ToastBreadAsync(number);
        ApplyButter(toast);
        ApplyJam(toast);
        return toast;
    }
}
```

中间的任务判断可以用switch代替，还以使用await Task.WhenAll(eggTask, baconTask, toastTask);等待全部完成。

# 待处理问题

有些地方理解不到位，以后会继续**更新补充**。

参考资料：

***

1.  异步编程模式 [↩︎](https://blog.csdn.net/FliesOfTime/article/details/100282584#fnref1)
    
2.  C#异步编程の-------异步编程模型（APM） [↩︎](https://blog.csdn.net/FliesOfTime/article/details/100282584#fnref2)