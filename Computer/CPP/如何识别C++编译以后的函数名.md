## 如何识别C++编译以后的函数名（demangle）

C/C++语言在编译以后，函数的名字会被编译器修改，改成编译器内部的名字，这个名字会在链接的时候用到。如果用[backtrace](https://link.jianshu.com/?t=http://www.gnu.org/software/libc/manual/html_node/Backtraces.html)之类的函数打印堆栈时，显示的就是被编译器修改过的名字，比如说\_Z3foov 。 那么这个函数真实的名字是什么呢？

每个编译器都有一套自己内部的名字，这里只是针对linux下g++而言。 以下是基本的方法: 
1. 每个方法都是以\_Z开头，
2. 对于嵌套的名字（比如名字空间中的名字或者是类中间的名字,比如Class::Func）后面紧跟N ， 然后是各个名字空间和类的名字，每个名字前是名字字符的长度，再以E结尾。(如果不是嵌套名字则不需要以E结尾)

比如上面的\_Z3foov 就是函数foo() , v 表示参数类型为void . 
又如N:C:Func 经过修饰后就是 \_ZN1N1C4FuncE, 这个函数名后面跟参数类型。 如果跟一个整型，那就是\_ZN1N1C4FuncEi

另外在linux下有一个工具可以实现这种转换，这个工具是c++filt , 注意不是c++filter.

```javascript
xuyang@ubuntu15:~/blog$ c++filt _ZN1N1C4FuncEi
N::C::Func(int)
```


native: #05 pc 003f0bcb /system/lib/libart.so (_ZN3art25JniMethodEndWithReferenceEP8_jobjectjPNS_6ThreadE+30)

类或命名空间中的变量或函数：

以”\_ZN”开头，然后是各个空间和类的名字，每个名字前是名的字符长度，然后是变量/函数名的长度和变量/函数名，后面紧跟”E”，然后如果是函数则跟参数别名，如果是变量则什么都不用加。如上面代码中的：mangling::C1::C2::func(int i)改编后的符号是\_ZN8mangling2C12C24funcEi

*ZN*

*3art*

*25JniMethodEndWithReference*

\_EP8\_jobjectjPNS\_6ThreadE+30

## SIGABRT的可能原因
3种可能 
1、double free/free 没有初始化的地址或者错误的地址 
2、堆越界 
3、assert

ID:   虚拟机分配的唯一的线程ID,在Dalvik里，它们是从3开始的奇数。
 Tid：linux的线程ID号
 Stauts：线程状态，比较多，有下面的一些
 ​             running:  正在执行程序代码
 ​             sleeping：执行了Thread.sleep()
 ​             monitor：等待接受一个监听锁。
 ​             wait:：Object.wait()，等待被其他线程唤醒
 ​             native：正在执行native代码，
 ​             vmwait：等待虚拟机，（这个不是很懂，高手指教，这个状态在什么情况下发生）
 ​             zombie：线程在垂死的进程
 ​             init：线程在初始化（我们不可能看到）
 ​             starting：线程正在启动（我们不可能看到）
 utime：执行用户代码的累计时间
 stime：执行系统代码的累计时间
 name：线程的名字
 
```javascript
04-22 11:12:22.105  8084  8691 E art     : JNI ERROR (app bug): accessed deleted global reference 0x7fa
04-22 11:12:22.128  8084  8691 F art     : art/runtime/java_vm_ext.cc:470] JNI DETECTED ERROR IN APPLICATION: use of deleted global reference 0x7fa
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]     from android.hardware.usb.UsbRequest android.hardware.usb.UsbDeviceConnection.native_request_wait()
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470] "Usb Read Thread" prio=5 tid=22 Runnable
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   | group="main" sCount=0 dsCount=0 obj=0x32c46d30 self=0xdf4d8d00
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   | sysTid=8691 nice=0 cgrp=default sched=0/0 handle=0xc9c13920
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   | state=R schedstat=( 579270 485573 4 ) utm=0 stm=0 core=1 HZ=100
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   | stack=0xc9b11000-0xc9b13000 stackSize=1038KB
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   | held mutexes= "mutator lock"(shared held)
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   native: #00 pc 0034d971  /system/lib/libart.so (_ZN3art15DumpNativeStackERNSt3__113basic_ostreamIcNS0_11char_traitsIcEEEEiP12BacktraceMapPKcPNS_9ArtMethodEPv+128)
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   native: #01 pc 0032e131  /system/lib/libart.so (_ZNK3art6Thread9DumpStackERNSt3__113basic_ostreamIcNS1_11char_traitsIcEEEEbP12BacktraceMap+308)
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   native: #02 pc 00237cad  /system/lib/libart.so (_ZN3art9JavaVMExt8JniAbortEPKcS2_+848)
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   native: #03 pc 00238243  /system/lib/libart.so (_ZN3art9JavaVMExt9JniAbortFEPKcS2_z+66)
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   native: #04 pc 00331b59  /system/lib/libart.so (_ZNK3art6Thread13DecodeJObjectEP8_jobject+240)
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   native: #05 pc 003f0bcb  /system/lib/libart.so (_ZN3art25JniMethodEndWithReferenceEP8_jobjectjPNS_6ThreadE+30)
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   native: #06 pc 0046cbdb  /system/framework/arm/boot-framework.oat (Java_android_hardware_usb_UsbDeviceConnection_native_1request_1wait__+86)
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   at android.hardware.usb.UsbDeviceConnection.native_request_wait(Native method)
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   at android.hardware.usb.UsbDeviceConnection.requestWait(UsbDeviceConnection.java:272)
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   at com.ximmerse.io.usb.JavaUsbStream.run(JavaUsbStream.java:196)
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   at android.os.Handler.handleCallback(Handler.java:751)
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   at android.os.Handler.dispatchMessage(Handler.java:95)
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   at android.os.Looper.loop(Looper.java:154)
04-22 11:12:22.129  8084  8691 F art     : art/runtime/java_vm_ext.cc:470]   at android.os.HandlerThread.run(HandlerThread.java:61)
```



### 参考链接

1.  [关于Android中so的符号表导出以及C++的符号改编规则](https://link.jianshu.com/?t=http://blog.csdn.net/Arno_W/article/details/50937515)
2.  [Android下打印调试堆栈方法](https://link.jianshu.com/?t=http://blog.csdn.net/freshui/article/details/9456889)
3.  [Coredump介绍及如何在Android中开启和使用来分析Crash等问题，coredumpandroid](https://link.jianshu.com/?t=http://www.bkjia.com/Androidjc/1009629.html)