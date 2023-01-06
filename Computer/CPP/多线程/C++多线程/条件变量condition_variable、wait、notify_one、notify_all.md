# 条件变量condition\_variable

std::condition\_variable实际上是一个和条件相关的类（等待一个条件达成），需要和互斥量配合使用，使用时要生成一个这个类的对象。

```cpp
std::condition_variable my_cond;
```

然后通过使用my\_cond的成员函数wait、notify\_one、notify\_all来进行互斥量的操作。

```cpp
std::unique_lock<std::mutex> myguard1(my_mutex1);
my_cond.wait(myguard1, [this] {if (!msgRecvQueue.empty()) return true; return false; });
```

上述代码中my\_cond.wait()的第一个参数是unique\_lock对象myguard1，第二个参数是一个lambda表达式，如果满足条件就返回true，不满足条件就返回false。

*   如果第二个参数返回值是true，那么wait()就会直接返回
*   如果第二个参数返回值是false，那么wait()就会解锁互斥量myguard1，并将该线程堵塞到本行

如果wait()堵塞到本行，那么可以通过其他线程使用my\_cond.notify\_one()来唤醒wait()，唤醒之后wait()会尝试再次拿锁并向下执行。

# notify\_one和notify\_all

notify\_one只会唤醒一个线程的wait()，而如果有多个线程需要等待唤醒，那么需要使用notify\_all()来唤醒所有线程中的wait()。