<h1>C#异步编程：TPL与Async&Await</h1>
# 一、TPL基础

线程池所提供的抽象帮助程序员封装了线程使用的细节，使我们可以专心处理程序逻辑，而不是各种线程问题；

.Net4.0引入了TPL，并在4.5进行了进一步完善；任务并行库(Task Parallel Library)可以被认为是线程池之上的又一个抽象层，它对程序员隐藏了与线程池交互的底层代码，并提供了更方便的细粒度的API；C#5.0已经内置了对TPL的支持，允许开发者使用新的await和async关键字以平滑、舒服的方式操作任务；

TPL的核心概念是Task（任务），一个任务代表了一个异步操作，该操作可以通过多种方式运行，可以使用或不适用独立线程运行；在默认情况下，开发者不需要知道任务实际上是如何执行的，TPL通过向用户隐藏任务的实现细节而创建一个抽象层；

组合任务：一个任务可以通过多种方式可以和其它任务组合起来，例如，可以同时启动多个任务，等待所有任务完成，然后运行一个任务对之前所有任务的结果进行一些计算，TPL与之前的模式相比，其中一个关键优势是其具有用于组合任务的便利的API；

## 创建任务

```
        static void Main(string[] args)
        {
            var t1 = new Task(() => TaskMethod("Task 1"));
            var t2 = new Task(() => TaskMethod("Task 2"));
            t2.Start();
            t1.Start();
            Task.Run(() => TaskMethod("Task 3"));
            Task.Factory.StartNew(() => TaskMethod("task 4"));
            Task.Factory.StartNew(() => TaskMethod("task 5"), TaskCreationOptions.LongRunning);
            Thread.Sleep(TimeSpan.FromSeconds(1));
        }

        static void TaskMethod(string name) {
            Console.WriteLine($"Task {name} is running on a theread id {Thread.CurrentThread.ManagedThreadId}");
        }
```

代码解释：

在程序运行时，使用Task的构造函数创建了两个任务，传入一个lambda表达式作为Action委托，这可以让我们调用TaskMethod并提供一个string参数，然后调用Start方法来执行任务（必须调用Start方法才能执行任务）；

然后使用Task.Run和Task.Factory.StartNew方法来运行了另外两个任务，与使用Task构造函数不同之处在于这两个被创建的任务会立即开始工作，所以无需显式调用这些任务的Start方法。从Task1到Task4（注意不包括Task5）的所有任务都被放置在线程池的工作线程中并以未指定的顺序运行，如果多次运行该程序，会发现任务执行顺序是不确定的。

Task.Run方法只是Task.Factory.StartNew的一个轻量版，后者有附加的参数选项，如Task5，标记了该任务为长时间运行，那么该任务将不会使用线程池，而是在单独的程序中运行。

## 使用任务执行基本的操作

```
        static void Main(string[] args)
        {
            TaskMethod("Main Thread Task");
            Task<int> task = CreateTask("Task 1");
            task.Start();
            int result = task.Result;
            Console.WriteLine($"Result is : {result}");

            task = CreateTask("Task 2");
            task.RunSynchronously();
            result = task.Result;
            Console.WriteLine($"Result is : {result}");

            task = CreateTask("Task 3");
            Console.WriteLine(task.Status);
            task.Start();

            while (!task.IsCompleted) {
                Console.WriteLine(task.Status);
                Thread.Sleep(TimeSpan.FromSeconds(0.5));
            }

            Console.WriteLine(task.Status);
            result = task.Result;
            Console.WriteLine($"Result is : {result}");
        }

        static Task<int> CreateTask(string name) {
            return new Task<int>(() => TaskMethod(name));
        }

        static int TaskMethod(string name) {
            Console.WriteLine($"Task {name} is running on a theread id {Thread.CurrentThread.ManagedThreadId}");
            Thread.Sleep(TimeSpan.FromSeconds(2));
            return 42;
        }
```

 代码解释：

首先直接运行TaskMethod方法，这里并没有把它封装到一个任务中，根据结果我们可以得到它是在主线程上被同步执行的，很显然它不是线程池中的线程；

然后运行了Task1，使用Start方法启动该任务并等待结果，该任务会被放置在线程池中，并且主线程会等待，知道任务返回前一直处于阻塞状态；

Task2和Task1类似，但是Task2是通过RunSynchronously()方法来运行的，这是一个同步运行的api，因此task2运行在主线程上；这个API是一个很好的优化，可以避免使用线程池来执行非常短暂的操作；

然后以运行Task1相同的方式运行Task3，这次没有阻塞主线程，只是在该任务完成前循环打印出任务状态，Created，Running，RanToCompletion;

## 组合任务

```
        static void Main(string[] args)
        {
            var firstTask = new Task<int>(() => TaskMethod("First Task", 3));
            var secondTask = new Task<int>(() => TaskMethod("Second Task", 2));
            firstTask.ContinueWith(t => Console.WriteLine($"the first answer is{t.Result}," +
                $"thread id {Thread.CurrentThread.ManagedThreadId}" +
                $",is thread pool thread:{Thread.CurrentThread.IsThreadPoolThread}"), TaskContinuationOptions.OnlyOnRanToCompletion);
            firstTask.Start();
            secondTask.Start();

            Thread.Sleep(TimeSpan.FromSeconds(4));
            Task continuation = secondTask.ContinueWith(t => Console.WriteLine($"the second answer is{t.Result}," +
                $"thread id {Thread.CurrentThread.ManagedThreadId}" +
                $",is thread pool thread:{Thread.CurrentThread.IsThreadPoolThread}"),
                TaskContinuationOptions.OnlyOnRanToCompletion | TaskContinuationOptions.ExecuteSynchronously);
            continuation.GetAwaiter().OnCompleted(() => Console.WriteLine($"continuation task completed" +
                $"thread id {Thread.CurrentThread.ManagedThreadId}" +
                $",is thread pool thread:{Thread.CurrentThread.IsThreadPoolThread}"));
            Thread.Sleep(TimeSpan.FromSeconds(2));
            Console.WriteLine();

            firstTask = new Task<int>(() => {
                var innerTask = Task.Factory.StartNew(() => TaskMethod("second task", 5), TaskCreationOptions.AttachedToParent);
                innerTask.ContinueWith(t => TaskMethod("Third Task", 2), TaskContinuationOptions.AttachedToParent);
                return TaskMethod("First Task", 2);
            });
            firstTask.Start();

            while (!firstTask.IsCompleted) {
                Console.WriteLine(firstTask.Status);
                Thread.Sleep(TimeSpan.FromSeconds(0.5));
            }
            Console.WriteLine(firstTask.Status);
            Thread.Sleep(TimeSpan.FromSeconds(10));
        }

        static int TaskMethod(string name, int seconds) {
            Console.WriteLine($"Task {name} is running on a theread id {Thread.CurrentThread.ManagedThreadId}");
            Thread.Sleep(TimeSpan.FromSeconds(seconds));
            return 42 * seconds;
        }
```

 代码解释：

创建了两个任务firsttask和secondtask，然后为firsttask设置了一个后续操作，使用ContinueWith；然后启动这两个任务并等待4秒，这个时间足够两个任务完成，然后第二个任务运行另一个后续操作，并通过指定同步选项来尝试同步执行该后续操作，如果后续操作耗时非常短暂，使用同步方式更高效，因为放置在主线程中运行比放置在线程池中运行要快。后面，也为之前的后续操作定义了一个后续操作，不过这里使用的是GetAwaiter和OnCompleted方法；最后的部分是父子线程，通过TaskCreationOptions.AttachedToParent选项来运行一个子任务，子任务必须在父任务运行时创建，并正确附加给父任务，并且只有所有子任务结束工作，父任务才能完成。

# 二、使用Await和Async的编程范式

在异步编程是，程序的实际执行顺序是比较难理解的，因为大型程序有许多相互依赖的任务和后续操作，比如用于运行其它后续操作的后续操作，处理异常的后续操作等；

C#引入了新的语言特性，即异步函数(Asynchronous Function)，它是TPL之上更高级别的抽象，真正简化了异步编程。抽象隐藏了主要的实现细节，使开发者无需考虑许多重要的事情，从而使异步编程更容易。

要创建一个异步函数，首先要使用async关键字标注一个方法；而且，异步函数必须返回Task或Task<T>类型，可以使用async void方法，但是更推荐使用async Task方法；

```
        async Task<string> GetStringAsync() {
            await Task.Delay(TimeSpan.FromSeconds(2));
            return "hello world";
        }
```

使用async关键字标注的方法内部，**可以使用await操作符，该操作符可与TPL的任务一起工作，并获取该任务中异步操作的结果**；而在async方法外不能使用await关键字，否则会有编译错误，另外，异步函数在其代码中至少要拥有一个await操作符，会有编译警告；

需要注意的是，以上代码在执行完await调用的代码行后会立即返回，如果是同步执行，执行线程将会阻塞两秒然后返回结果。这里当执行完await操作后，立即将工作线程放回线程池的过程中，我们会异步等待，2秒后，我们有一次从线程池中得到工作线程继续运行其中剩余的异步方法；借助于异步函数，我们拥有了线性的程序控制流，但它的执行依然是异步的。

异步函数会被C#编译器在后台编译成复杂的程序结构，生成的代码和另一个C#构造很类似，也就是迭代器，生成的代码被实现为一种状态机。尽管很多程序员几乎开始为每个方法使用async修饰符，但是如果方法本来无需异步或并行执行，那么将方法标注为async是不可取的，因为async方法会有显著的性能损失。

## 使用await操作符获取异步任务结果

```
        static void Main(string[] args)
        {
            Task t = AsynchronyWithTPL();
            t.Wait();

            t = AsynchronyWithWait();
            t.Wait();
        }

        static Task AsynchronyWithTPL() {
            Task<string> t = GetInfoAsync("Task 1");
            Task t2 = t.ContinueWith(task => Console.WriteLine(t.Result), TaskContinuationOptions.NotOnFaulted);
            Task t3 = t.ContinueWith(task => Console.WriteLine(t.Exception.InnerException), TaskContinuationOptions.OnlyOnFaulted);
            return Task.WhenAny(t2, t3);
        }

        async static Task AsynchronyWithWait() {
            try
            {
                string result = await GetInfoAsync("Task 2");
                Console.WriteLine(result);
            }
            catch (Exception ex) {
                Console.WriteLine(ex);
            }
        }

        async static Task<string> GetInfoAsync(string name) {
            await Task.Delay(TimeSpan.FromSeconds(2));
            return string.Format($"Task {name} is running on a thread id{Thread.CurrentThread.ManagedThreadId}" +
                $" Is Thread Pool thread:{Thread.CurrentThread.IsThreadPoolThread}");
        }
```

 代码解释：

在main里运行了两个功能相同的异步操作，其中第一个异步操作是标准的TPL模式的代码，第二个是使用了C#的新特性async和await。AsynchronyWithTPL方法启动了一个任务，运行两秒后返回一个字符串，然后我们定义了一个后续操作用于打印出结果字符串；而在AsynchronyWithAwait方法中，我们对任务使用await并得到了相同的结果，这和编写同步代码的风格一样，即我们获取任务的结果，打印出结果，如果有异常则捕捉异常；关键不同的是这实际是一个异步程序，使用await后，C#立即创建了一个任务，其有一个后续任务，包含了await操作符后面的所有剩余代码。

## 在lambda表达式中使用await操作符

```
        static void Main(string[] args)
        {
            Task t = AsynchronousProcessing();
            t.Wait();
        }

        async static Task AsynchronousProcessing() {
            Func<string, Task<string>> asyncLambda = async name => {
                await Task.Delay(TimeSpan.FromSeconds(2));
                return string.Format($"Task {name} is running on a thread id{Thread.CurrentThread.ManagedThreadId}" +
                    $" Is Thread Pool thread:{Thread.CurrentThread.IsThreadPoolThread}");
            };
            string result = await asyncLambda("async lambda");
            Console.WriteLine(result);
        }
```

代码解释：

由于不能在main方法中使用async，所以将异步函数移到了AsynchronousProcessing方法中，然后使用async关键字声明了一个lambda表达式，这里定义了lambda表达式体，它被定义返回一个Task<string>对象，但实际返回的是字符串，却没有编译错误，这是因为C#编译器自动创建了一个任务并返回；

## 对连续的异步任务使用await操作符

```
        static void Main(string[] args)
        {
            Task t = AsynchronyWithWait();
            t.Wait();
        }

        async static Task AsynchronyWithWait()
        {
            try
            {
                string result = await GetInfoAsync("Async 1");
                Console.WriteLine(result);
                result = await GetInfoAsync("Async 2");
                Console.WriteLine(result);
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex);
            }
        }

        async static Task<string> GetInfoAsync(string name)
        {
            await Task.Delay(TimeSpan.FromSeconds(2));
            if (name == "TPL2") {
                throw new Exception("Boom");
            }
            return string.Format($"Task {name} is running on a thread id{Thread.CurrentThread.ManagedThreadId}" +
                $" Is Thread Pool thread:{Thread.CurrentThread.IsThreadPoolThread}");
        }
```

代码解释：

在AsynchronyWithAwait内，使用了两个await声明，最重要的是该代码依然是顺序执行的，async2任务只有等之前的任务完成后才会开始执行，当阅读这部分代码时，程序流很清晰，可以看到什么先运行，什么后运行。但是这个程序不总是异步的额，使用await时如果一个任务已经完成，我们可以异步得到该任务结果，否则，当在代码中看到await声明时，通常的行为是方法执行到await代码时将立即返回，剩下的代码会在一个后续操作任务中运行，因此等待操作结果时并没有阻塞程序的运行，这是一个异步调用；