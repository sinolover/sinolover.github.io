　　python中的装饰器是一个用得非常多的东西，我们可以把一些特定的方法、通用的方法写成一个个装饰器，这就为调用这些方法提供一个非常大的便利，如此提高我们代码的可读性以及简洁性，以及可扩展性。

在学习python装饰器之前我们先看看这样一个例子：

一、作用域
~~~python
# coding:utf-8
 
msg = 'hello test1'
 
 
def add():
    msg = 'this is add'
    print msg     #当执行add()时将打印'this is add'
def add2():
    print msg     #当执行add2()时将打印'hello test1'
 ~~~
    上面的例子简单地对python的作用域做了一个说明，例子中申明了全局变量msg,add函数中也申明了一个局部变量msg，当执行add()的 "print msg" 时会先找add里面是否有局部变量msg，如果没找到就去上一级作用域找是否存在该变量，局部变量在函数运行时生成，当函数结束运行时局部变量也随之销毁，接下来我们对上面的例子加深：

二、闭包
~~~python
# coding:utf-8
 
def add():
    msg = 'hello add'
    def inner():
        print msg               #运行add()这里会打印'hello add'
    return inner
>>> obj = add()
>>> obj            #可见obj是一个指向inner函数的对象
<function inner at 0x0000000002855438>
...
>>> obj()
hello add           #运行obj()打印msg
~~~
　　看了上面的例子有没有一点疑惑，obj是指向inner函数的对象，当运行obj时就等同于运行inner，但是add函数并没有运行，也就是说msg没有被申明同时inner也没有申明变量msg,它又是如何找到msg这个变量的值呢？

　　这就是python里的"闭包",Python支持一个叫做函数闭包的特性，用人话来讲就是，嵌套定义在非全局作用域里面的函数能够记住它在被定义的时候它所处的封闭命名空间。这能够通过查看函数的obj.func_closure属性得出结论，这个属性里面包含封闭作用域里面的值（只会包含被捕捉到的值，如果在add里面还定义了其他的值，封闭作用域里面是不会的）闭包就是python装饰器的核心原理，接下来我们写一个简单的装饰器例子：

三、简单的装饰器
~~~python
# coding:utf-8
 
def check_args_num(func):
    # 该装饰器用于检查传入的参数数量，检查是否是两个参数
    def inner(*args, **kwargs):
        args_list = list(args)
        if len(args_list) < 2:
 
            for i in range(2 - len(args)):
                # 如果参数数量小于2则用0填补
                args_list.append(0)
        if len(args_list) > 2:
            # 如果参数数量大于2则打印错误
            print 'The args number is too many!'
        func(*args_list, **kwargs)
    return inner
 
 
@check_args_num
def add(x, y):
    return x + y
~~~
执行结果：

~~~python
>>>print add(1,2)
3
...
>>>print add(100)
100
...
>>>print add(1,2,3)
Traceback (most recent call last):
  File "D:\PyCharm 5.0.4\helpers\pydev\pydevd_exec.py", line 3, in Exec
    exec exp in global_vars, local_vars
  File "<input>", line 1, in <module>
  File "E:/code/my_project/decorator/test1.py", line 14, in inner
    raise Exception('The args number is too many!')
Exception: The args number is too many!
...
>>>add
<function inner at 0x0000000002A6C3C8>
#可以看到add函数现在指向的是inner
~~~
四、多个装饰器
~~~python
# coding:utf-8
 
def check_args_int(func):
    # 该装饰器用于检查传入的参数是否是int型
    def ensure_int(*args, **kwargs):
        from array import array
        try:
            array('H', args)
        except Exception, e:
            raise Exception(e)
        return func(*args, **kwargs)
 
    return ensure_int
 
 
def check_args_num(func):
    # 该装饰器用于检查传入的参数数量，检查是否是两个参数
    def inner(*args, **kwargs):
        args_list = list(args)
        if len(args_list) < 2:
 
            for i in range(2 - len(args)):
                # 如果参数数量小于2则用0填补
                args_list.append(0)
        if len(args_list) > 2:
            # 如果参数数量大于2则打印错误
            raise Exception('The args number is too many!')
        return func(*args_list, **kwargs)
 
    return inner
 
 
@check_args_num
@check_args_int
def add(x, y):
    return x + y
~~~
这里多加了一个参数内容检查的装饰器，当多个装饰器时，会自上而下执行，先执行check_args_num再执行check_args_int,执行结果：

~~~python
>>> print add(1,'fsaf')
Traceback (most recent call last):
  File "D:\PyCharm 5.0.4\helpers\pydev\pydevd_exec.py", line 3, in Exec
    exec exp in global_vars, local_vars
  File "<input>", line 1, in <module>
  File "E:/code/my_project/decorator/test1.py", line 28, in inner
    return func(*args_list, **kwargs)
  File "E:/code/my_project/decorator/test1.py", line 10, in ensure_int
    raise Exception(e)
Exception: an integer is required
...
>>> add
<function inner at 0x0000000002B1C4A8>
#这里可以看到add还是指向inner,我们可以这样理解当多个装饰器存在时，当调用add时，其调用入口始终是第一个装饰器，第一个装饰器执行完，再执行下一个，同时也会将参数依次传递下去
~~~
五、带参数的装饰器
我们知道定义装饰器的时候传入装饰器的第一个参数是被装饰的函数(eg.例子中的add),有些时候我们也需要给装饰器传递额外的参数，下面例子将给装饰器传递额外参数，如下：
~~~python

# coding:utf-8
 
def check_args_int(func):
    # 该装饰器用于检查传入的参数是否是int型
    def ensure_int(*args, **kwargs):
        from array import array
        try:
            array('H', args)
        except Exception, e:
            raise Exception(e)
        return func(*args, **kwargs)
 
    return ensure_int
 
 
def check_args_num(flag):
    '''
    :param func: 被装饰函数
    :param flag: 决定是否检查参数数量
    '''
 
    # 该装饰器用于检查传入的参数数量，检查是否是两个参数
    def get_func(func):
 
        def inner(*args, **kwargs):
            if flag == 'false':
                print 'Skip check !'
                return func(*args, **kwargs)
            args_list = list(args)
            if len(args_list) < 2:
 
                for i in range(2 - len(args)):
                    # 如果参数数量小于2则用0填补
                    args_list.append(0)
            if len(args_list) > 2:
                # 如果参数数量大于2则打印错误
                raise Exception('The args number is too many!')
            return func(*args_list, **kwargs)
 
        return inner
 
    return get_func
 
 
@check_args_num('false')
@check_args_int
def add(x, y):
    return x + y
~~~
这个例子只有check_args_num和之前的有所不同，不同在于装饰器check_args_num多了一个参数flag，当flag=='false'时，跳过参数数量检查，下面是输出结果

~~~python
>>>print add(1, 2)
Skip check !
3
~~~
六、decorator不带参数的装饰器
　　decorator模块是python用来专门封装装饰器的模块，使用decorator构造装饰器更加简便，同时被装饰的函数签名也保留不变
　　之前的讲了通过闭包实现python装饰器构造，这里利用decorator模块实现python装饰器与其原理都是一样的
~~~python
from decorator import decorator
 
 
@decorator
def check_num(func, *args, **kwargs):
    if len(args) != 2:
        raise Exception('Your args number is not two')
    else:
        return func(*args, **kwargs)
 
 
@check_num
def add(x, y):
    return x + y
　　

>>> add
<function add at 0x0000000002D43BA8>
>>> print add(1,2)
3
...
>>> add(1,2,3)
Traceback (most recent call last):
  File "D:\PyCharm 5.0.4\helpers\pydev\pydevd_exec.py", line 3, in Exec
    exec exp in global_vars, local_vars
  File "<input>", line 1, in <module>
TypeError: add() takes exactly 2 arguments (3 given)
 
#可以看到这里当我们传三个参数给add()函数时，他直接从add()函数抛出类型错误异常，
#并没有进入check_num装饰器进行参数校验，可见被装饰的函数add()的签名还是他本身
#如果直接构造装饰器，那么这里将会从check_num里面抛出异常，如下：
 
def check_num(func):
    def inner(*args,**kwargs):
        if len(args) != 2:
            raise Exception('Your args number is not two')
        else:
            return func(*args,**kwargs)
    return inner
 
@check_num
def add(x, y):
    return x + y
 
 
>>> add
<function inner at 0x0000000002E233C8>
>>>add(1,2,3)
Traceback (most recent call last):
  File "D:\PyCharm 5.0.4\helpers\pydev\pydevd.py", line 2411, in <module>
    globals = debugger.run(setup['file'], None, None, is_module)
  File "D:\PyCharm 5.0.4\helpers\pydev\pydevd.py", line 1802, in run
    launch(file, globals, locals)  # execute the script
  File "E:/code/my_project/decorator/test3.py", line 14, in <module>
    print add(1,2,3)
  File "E:/code/my_project/decorator/test3.py", line 4, in inner
    raise Exception('Your args number is not two')
Exception: Your args number is not two
~~~
从上面的例子可以看出，通过闭包的方式构造装饰器时，其执行函数入口是装饰器中的嵌套函数，这样就可能出现上面出现的问题，当add(1,2,3)执行时会先执行inner函数（如果inner里面没有参数校验则这里是不会抛出异常的，只有当执行到return func(*args,**kwargs)时才真正调用add(x,y)函数，这时候才会抛出异常）这样就会造成程序执行多余的代码，浪费内存，cpu。

七、decorator带参数的装饰器
如果想让装饰器带参数呢？

~~~python
from decorator import decorator
 
 
def check(flag):
    @decorator
    def check_num(func, *args, **kwargs):
        if flag == 'false':
            print 'skip check !'
            return func(*args,**kwargs)
        if len(args) != 2:
            raise Exception('Your args number is not two')
        else:
            return func(*args, **kwargs)
    return check_num
 
 
@check('false')
def add(x, y):
    return x + y
 
>>>add
<function add at 0x0000000002C53C18>
>>>add(1,2)
skip check !
3
~~~
　　decorator 模块就讲到这里，这个模块比较简单，还有一些功能没讲到的查看源码便一目了然，其原理都是利用python闭包实现的

 八、functools.wraps(func)装饰器
 　　functools.wraps和decorator模块的功能是一样的，都是为了解决被装饰函数的签名问题，这里只对该种装饰器构造方法列举一个带参数例子：

~~~python
import functools
 
def check(flag):
    def wraps(func):
        @functools.wraps(func)
        def check_num(*args, **kwargs):
            if flag == 'false':
                print 'skip check !'
                return func(*args,**kwargs)
            if len(args) != 2:
                raise Exception('Your args number is not two')
            else:
                return func(*args, **kwargs)
        return check_num
    return wraps
 
@check('false')
def add(x, y):
    return x + y
~~~
　　对比上面通过decorator模块装饰函数的例子，我们可以发现，用decorator装饰函数的代码更加简洁易懂，但是他们二者的执行效率谁更高呢？下面我们通过Timer来测试下：

~~~python
from timeit import Timer
 
print Timer('add(1,2)',setup='from __main__ import add').timeit(100000)
 
#将该段代码 加在 之前的例子中
#这里打印的是运行100000次的时间
~~~
　　functools.wraps装饰执行结果：
~~~python
1
2.37299322602
~~~
　　decorator模块装饰执行结果：
~~~python
1
3.42141566059
~~~
　　执行效率wraps略高，但是这里是执行了10万次他们之间的差距约为1秒，所以我个人还是比较青睐于用decorator模块装饰函数，毕竟看起来易懂，写法也较为简单！本文就将装饰器介绍到这里了，当然也没有说尽装饰器的妙用，比如：装饰类...其原理是用类来当做装饰器，类里面需要用到__call__方法，至于装饰类的用法感兴趣的朋友自行百度咯！

 