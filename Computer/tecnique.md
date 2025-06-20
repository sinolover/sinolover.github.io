# 框架是和库的区别
框架是和库相对的。简单的区分是使用框架时，计算机先运行框架，然后框架会在合适的时候调用你写的代码，也就是你新写的代码是对原有框架的扩展，框架是程序逻辑的骨架。而使用库时，你会写代码负责调用库里提供的功能，你的代码才是程序逻辑的骨架。一个形象的例子就是建筑物，一个建筑物会有钢筋混凝土或其它材质的骨架作为框架，在框架的基础上还会有不同的附属设施以实现不同的功能，就像使用库代码一样。作为一个建筑工人，你不能破坏已经建好的框架基础，但你还是可以自由的选择贴哪种壁纸接上什么型号的电灯。

# 问答
## 问下if else多层嵌套，实际开发中，可以怎么优化？
分很多种情况：
1、原则上尽量减少else的出现，目的是减少逻辑嵌套，让代码更易读
2、在1的逻辑基础上。如果分支内有返回值，优先将有返回值的if提取出来放在前面（尽可能早的退出嵌套）
3、在2的基础上，如果返回值是一个对象，可以使用工厂模式和责任链模式代替if else
4、如果没有返回值，在1的基础上用三元表达式、重开方法包装if分支的逻辑
5、其他的情况还是要看实际开发的业务逻辑，有的时候业务逻辑本身可以优化的话，代码中ifelse也会减少

# 闭包

闭包相当于定义了一个微小的类。
类的属性就是闭包被访问的外部变量，类的访问类属性的非静态方法就是闭包函数。
每次调用闭包生成函数的过程，是创建实例的过程，初始化了实例中的属性
而类的静态方法，不能使用非静态的属性的原理就在这里。
闭包不能直接通过点访问内部属性，只能通过闭包函数间接访问，从而实现了对属性的封装。类似Java中的private属性+getter setter方法访问控制。所以闭包在纯面向对象中用处较小。
js在ES6之前实现类的方法，其中之一，就是通过类实现的。
在脚本语言中，定义一个类代价太过高昂，而灵活的闭包却可以方便的实现。
好的程序就是在可大可小的工具中自由穿梭而运行稳定，表述清晰。我们像这样的目标进军


# 有状态对象
在很多情况下， 一个对象的行为取决于一个或多个动态变化的属性 ，这样的属性叫做 状态 ，这样的对象叫做 有状态的 ( stateful ) 对象 ，这样的对象状态是从事先定义好的一系列值中取出的。当一个这样的对象与外部事件产生互动时，其内部状态就会改变，从而使得系统的行为也随之发生变化。
# 静态类 封闭类
静态类 不能实例化，不能有实例变量，实例方法
封闭类 不能继承
扩展方法 语法
 static className{
    public static  methodName(this ...)
}
注意
1.扩展方法所在的类必须是静态类
2.扩展方法的第一个参数类型是被扩展的类型，该类型定义的变量的用法相当于this指针。同时，类型前面标注this
3.使用自定义的扩展方法的代码必须添加对该扩展方法所在类namespace进行using
4.扩展方法最终其实还是被编译器处理成静态普通方法的调用。是一个语法糖
5.扩展方法由于本质上还是静态方法的调用，所以不能访问类外部访问不了的成员，例如private protected 成员

# 多线程
每个进程中访问临界资源的那段代码称为临界区(Critical Section)(临界资源是一次仅允许一个进程使用的共享资源)。每次只准许一个进程进入临界区，进入后不允许其他进程进入。不论是硬件临界资源，还是软件临界资源，多个进程必须互斥地对它进行访问。

多个进程中涉及到同一个临界资源的临界区称为相关临界区。.

# Azure900
地区 区域对 区域 可用性区域-带白点的
可用性集 故障域 更新域

存储服务
结构化 半结构化
Cosmos db sqlserver
非结构化
Blob  磁盘 文件共享 消息队列

# IFD面向互联网部署
Internet-Facing Deployment
FS联合身份验证 
Federated Identity

# 电脑安装后初始设置
## 一、系统路径修改win7设置桌面位置 
1、新系统安装时： 
 在安装Win7的过程中，要求输入用户名及密码的时候，先不如输入任何信息，按“Shift+F10”呼出DOS窗口，输入以下命令（不分大小写）：
 robocopy \"C:\\Users\" \"E:\\Users\" /E /COPYALL /XJ /XD  
 rmdir \"C:\\Users\" /S /Q  
 mklink /J \"C:\\Users\" \"E:\\Users\" 
 关闭DOS窗口，继续安装直至完成。
2、系统激活
windows10 LTSC 2019 激活以管理员权限打开cmd依次输入 ：
slmgr -ipk M7XTQ-FN8P6-TTKYV-9D4CC-J462D
slmgr -skms kms.03k.orgslmgr -atoslmgr -dlv
## 二、系统配置
1. 关闭系统声音
2. 关自动更新 gpedit.msc computer->administrator template -> windows ->...    
Registry
HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\WaaSMedicSvc，右侧FailureActions键，右键点击修改该键的二进制数据，将“0010”、“0018”行的左起第5个数值由原来的“01”改为“00”，START改为4
把wuauserv（Windows Update） UsoSvc（Update Orchestrator Service） BITS（Background Intelligent Transfer Service）这三个服务也操作一遍
taskschd.msc
3. 关闭Hiberenate powercfg -h off
4. 修改Chrome 快捷方式 添加 参数：
        设置缓存目录                          设置缓存大小                           设置配置文件          
        -disk-cache-dir=\"O:/temp\" -disk-cache-size=104857600  -profile-directory=\"Default\"  
5. 创建快捷方式 shutdown -s -t 0  ，并设置快捷键： ctrl + alt + shift + s
6. 打开桌面图标 我的电脑 网上邻居 等

# ANI AGI ASI
人工智能可以分为：
弱人工智能(Artificial Narrow Intelligence，简称ANI)
强人工智能(Artificial General Intelligence简称AGI)
超人工智能(Artificial Superintelligence简称ASI)三个等级。

# ABI(Application Binary Interface): 
应用程序二进制接口 描述了应用程序和操作系统之间，一个应用和它的库之间，或者应用的组成部分之间的低接口。

# 右值
左值特点：有可能：有名，可取址，能存值（具名，能存值，可取址）
具名，可址，可值
左值，可以放在等号左边的值，特点：有名字，能够取址，可以存值

# Unicode
Unicode是一种字符集，而UTF-8是Unicode的一种实现方式。此外，Unicode定义了每个字符的编号和名称，而UTF-8则是一种用于在计算机上存储和传输Unicode字符的编码方式
Unicode编码范围是从U+00_0000到U+10_FFFF（十六进制）
Unicode编码方案定义了17个平面（Plane）：0x00-0x10，每个平面包含65,536个码位：FFFF两个字节，总共有1,114,112个码位。名列前茅个平面（Plane 0，也称为基本多文种平面（Basic Multilingual Plane，BMP））包含了大部分常用的字符和符号，包括ASCII字符集和大部分欧洲语言中的字符。其他平面包含了一些罕见的字符和符号，例如古文字、符号和表情符号等。

# string
概括一下strlen、strcpy/strncpy、strcat/strncat、strcmp/strncmp、strchr/strrchr、strstr、strtok、atoi/atol/atof/itoa/ltoa/ftoa

rpath : runtime library directory


# c++运算符优先级
乘除余加减，移位比较，等于不等于，位逻辑

乘除余加减，移位再比较，等于不等于，先位后逻辑

野指针，未初始化的，不知道指向哪里，所以野
悬空指针，被释放，未置空的。明确知道指向哪个地址，只是该地址的内存已经还给操作系统了
