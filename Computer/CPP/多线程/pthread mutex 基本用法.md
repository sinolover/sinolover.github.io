转自：[pthread mutex 基本用法 | feng 言 feng 语](https://feng-qi.github.io/2017/05/08/pthread-mutex-basic-usage/ "pthread mutex 基本用法 | feng 言 feng 语")

锁是程序中经常需要用到的机制，尤其是多线程的程序中，如果没有锁的帮助，线程间的同  
步就会非常麻烦甚至不可能。`pthread`中提供了`mutex`互斥量这种锁，在 linux 下经常  
用到，以下是`pthread_mutex_t`的相关函数介绍及简单用法。

# 相关函数
~~~cpp
#include <pthread.h>

pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
int pthread_mutex_init(pthread_mutex_t *restrict mutex,
                       const pthread_mutexattr_t *restrict attr);
int pthread_mutex_destroy(pthread_mutex_t *mutex);

int pthread_mutex_lock(pthread_mutex_t *mutex);
int pthread_mutex_trylock(pthread_mutex_t *mutex);
int pthread_mutex_unlock(pthread_mutex_t *mutex);
~~~
# 使用方法

使用`mutex`的基本步骤就是：

定义`mutex` -> 初始化`mutex` -> 使用`mutex`(lock, unlock, trylock) -> 销毁`mutex`。

函数名也已经把它自己的功能描述的非常清楚了。只是有一些细节需要注意：

`pthread_mutex_t`的初始化有两种方法，一种是使用函数`pthread_mutex_init`，使用结  
束需要调用函数`pthread_mutex_destroy`进行销毁，调用时`mutex`必须未上锁。

> It shall be safe to destroy an initialized mutex that is unlocked. Attempting  
> to destroy a locked mutex or a mutex that is referenced (for example, while  
> being used in a pthread\_cond\_timedwait() or pthread\_cond\_wait()) by another  
> thread results in undefined behavior.
> 
> – man pthread\_thread\_destroy

大意是如果`mutex`是上锁状态，或者被`pthread_cond_timedwait()`或`pthread_cond_wait()`  
函数引用，此时对其调用`pthread_mutex_destroy()`结果未定义。

第二种方法是使用`PTHREAD_MUTEX_INITIALIZER`。根据\[1\]中的描述，似乎对使用这种方法  
初始化的`mutex`调用`pthread_mutex_destroy()`会产生错误，对未上锁的`mutex`调用  
`pthread_mutex_unlock`也会产生错误。

> Mutex initialization using the PTHREAD\_MUTEX\_INITIALIZER does not immediately  
> initialize the mutex. Instead, on first use, pthread\_mutex\_lock() or  
> pthread\_mutex\_trylock() branches into a slow path and causes the  
> initialization of the mutex. Because a mutex is not just a simple memory  
> object and requires that some resources be allocated by the system, an attempt  
> to call pthread\_mutex\_destroy() or pthread\_mutex\_unlock() on a mutex that has  
> statically initialized using PTHREAD\_MUTEX\_INITIALER and was not yet locked  
> causes an EINVAL error.

但并没有找到太多佐证，也不知道这个系统与 Linux 上的实现是否相同。

# 例子程序
~~~cpp
#include <stdio.h>
#include <pthread.h>

/* pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER; */
pthread_mutex_t mutex;

int count;

void * thread_run(void *arg)
{
    int i;
    pthread_mutex_lock(&mutex);
    for (i = 0; i < 3; i++) {
        printf("[%#lx]value of count: %d\n", pthread_self(), ++count);
    }
    pthread_mutex_unlock(&mutex);
    return 0;
}

int main(int argc, char *argv[])
{
    pthread_t thread1, thread2;
    pthread_mutex_init(&mutex, 0);
    pthread_create(&thread1, NULL, thread_run, 0);
    pthread_create(&thread2, NULL, thread_run, 0);
    pthread_join(thread1, 0);
    pthread_join(thread2, 0);
    pthread_mutex_destroy(&mutex);
    return 0;
}
~~~
如果使用了第 4 行的初始化方法，可以删除 23 和 28 行。

# Reference

\[1\] [pthread\_mutex\_destroy()–Destroy Mutex - IBM Knowledge Center](https://www.ibm.com/support/knowledgecenter/en/ssw_i5_54/apis/users_60.htm "pthread_mutex_destroy()–Destroy Mutex - IBM Knowledge Center")