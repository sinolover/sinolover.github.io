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
