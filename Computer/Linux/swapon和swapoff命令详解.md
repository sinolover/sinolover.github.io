
# CentOS 7 在VM里修改SWAP遇到swapon failed invalid argument
本人之前安装CentOS 7学习Oracle的时候，发现初始安装配置的swap小了（因为懒，全部选择的默认安装 T。T），为了不浪费时间重装一次，就去找了下如何修改swap配置。

根据度娘出来的帖子内容，fallocate 创建新swap文件之后，mkswap 也很正常，然而，swapon 出现了问题
~~~
[root@oracle ~]# swapon /swapfile
swapon: /swapfile: swapon failed: Invalid argument
~~~
这个错误是fallocate造成的，<font color=red>如果使用dd 来创建新swap文件，mkswap之后就没有这个问题</font>
~~~
[root@oracle ~]# dd if=/dev/zero of=/swapfile bs=1024 count=4194304 
[root@oracle ~]# mkswap /swapfile
[root@oracle ~]# swapon /swapfile
[root@oracle ~]# free -m
              total        used free      shared  buff/cache   available
Mem: 3933        1524         124          48        2285        2038 Swap: 8191          16        8175
~~~

在GitHub上找到了关于这个问题的一个帖子，其中，作为fallocate的contributor，Andrew Gross也参与了讨论。https://github.com/sous-chefs/swap/issues/5

这是fallocate对于xfs支持的问题，根据Andrew Gross自己的测试，fallocate对于ext4各版本的支持都没什么问题，但是对于老版本xfs的支持则不那么好了。

然而在原帖里可以发现，实际上现在对于高版本的xfs也已经支持不那么好了，我的xfs版本如下。
~~~
[root@oracle ~]# yum list installed | grep xfs
xfsdump.x86_64 3.1.7-1.el7                    @anaconda
xfsprogs.x86_64 4.5.0-15.el7                   @anaconda
~~~
总结：

综上所述，遇到这种问题的时候，用dd吧，虽然时间是比fallocate要长一点。最好的解决办法，就是在安装和部署任何应用之前，还是把sizing提前做好吧。


[基础命令学习目录首页](https://www.cnblogs.com/machangwei-8/p/9299013.html)

原文链接：https://www.cnblogs.com/machangwei-8/p/10354464.html

# swapon命令 
**swapon命令**用于激活Linux系统中的交换空间，Linux系统的内存管理必须使用交换区来建立虚拟内存。

## 语法

```html
swapon(选项)(参数)
```

## 选项
~~~
1.  -a：将/etc/fstab文件中所有设置为swap的设备，启动为交换区；
    
2.  -h：显示帮助；
    
3.  -p<优先顺序>：指定交换区的优先顺序；
    
4.  -s：显示交换区的使用状况；
    
5.  -V：显示版本信息。
~~~    

## 参数

交换空间：指定需要激活的交换空间，可以是交换文件和交换分区，如果是交换分区则指定交换分区对应的设备文件。

## 实例
~~~
1.  mkswap -c /dev/hdb4 （-c是检查有无坏块）
    
2.  swapon -v /dev/hdb4
    
3.  swapon -s
    
4.  Filename type Size Used Priority
    
5.  /dev/hda5 partition 506008 96 -1
    
6.  /dev/hdb4 partition 489972 0 -2
~~~

# swapoff命令
用于关闭指定的交换空间（包括交换文件和交换分区）。swapoff实际上为[swapon](http://man.linuxde.net/swapon)的符号连接，可用来关闭系统的交换区。

## 语法

```html
swapoff(选项)(参数)
```

## 选项

```html
-a：关闭配置文件“/etc/fstab”中所有的交换空间。
```

## 参数

交换空间：指定需要激活的交换空间，可以是交换文件和交换分区，如果是交换分区则指定交换分区对应的设备文件。

## 实例

关闭交换分区

```html
swapoff /dev/sda2
```

# 扩展知识:
## 一、利用swapoff和swapon刷新swap缓存

有时运行大量的进程后swap大量占用，达到30%的话机器会变得很慢  
  
可以用以下两个命令清除刷新swap  
~~~  
swapoff -a  
swapon -a  
~~~ 
这样swap就还原到初始状态  
  
以下是设置swap优先级的方法  
  
swappiness  
Ubuntu Feisty默认的vm.swappiness值是60,这一默认值已经很合适了。但你可以改小一些降低swap的加载，系统性能会有一点点的提升  
输 入：  
 
~~~
sysctl -q vm.swappiness
~~~
  
你会看到值是60， 更改:  
 

[su](http://www.linuxso.com/command/su.html)do sysctl vm.swappiness=10

  
这 样你就将值由60改为10,这可以大大降低系统对于swap的写入，建议内存为512m或更多的朋友采用此方法。如你你发现你对于swap的使用极少，可 以将值设为0。这并不会禁止你对swap的使用，而是使你的系统对于swap的写入尽可能的少，同时尽可能多的使用你的实际内存。这对于你在切换应用程序 时有着巨大的作用，因为这样的话它们是在物理内存而非[swap分区](https://www.baidu.com/s?wd=swap%E5%88%86%E5%8C%BA&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)中。  
如果你想永久得改变这一值，你需要更改 sysctl.conf 文件：  
 
~~~
vim /etc/sysctl.conf
~~~
  
添加：  
`vm.swappiness=10`
到末行，需要重启生效。  
我发现对 于我的1G内存，将此值设为5是最合适的。

例：启用交换分区/dev/sda5。

`[root@rhel ~]# swapon /dev/sda5`

例：启用交换文件/swapfile。

`[root@rhel ~]# swapon /swapfile`

　　 swapon 是开启swap.  
　　相对的,便有一个关闭swap的指令,swapoff.

## 二、linux系统swap分区

swap分区是必须有的，首先，它是日志文件系统得以发挥作用的依赖，在系统意外关闭的情况下，靠它来保存系统中的数据。其次，在运行一些比较耗内存的程序的时候（比如p2p下载），也要用到它。在这两种情况之外，swap分区处于一种闲置状态，比如：  

~~~
free
             total       used       free     shared    buffers     cached  
Mem:        451436     213772     237664          0       6748     114248  
-/+ buffers/cache:      92776     358660  
Swap:       514040          0     514040  
~~~
这种情况是通常的情形，但我们不能因此忽视交换分区的重要作用。linuxso.com

我的两块硬盘各有一个swap分区，几个Linux共享这两个swap区，但用swapon -s检查swap分区时发现少了一个。于是运行：  
~~~
mkswap -c /dev/hdb4 （-c是检查有无坏块）  
swapon -v /dev/hdb4  
~~~
然后正常了：  
~~~
swapon -s
Filename                                Type            Size    Used    Priority  
/dev/hda5                               partition       506008 96      -1  
/dev/hdb4                               partition       489972 0       -2
~~~
由于系统建立的方式各异，交换分区有时候完全不需要手工mkswap和swapon（如正常的光盘安装或者网络安装），但有的时候需要简单地弄一下（比如借腹生子式的系统建立方式），如果syslog上面出现：  
mkswap /dev/hdb4 : Invalid argument 提示的时候，就需要经历一个mkswap的过程才行

## 三、简述创建swap虚拟内存的过程

**大概步骤**  
1. 新建一个分区 用fdisk /dev/sda 进去去new一个分区 具体不多说了 w保存  
然后partprobe 重新读入分区表  
2. 假设刚刚新建的分区为 /dev/sda6  
那么mkswap /dev/sda6  
3. swapon /dev/sda6 这样就可以了啊  
用free 查看一下 就能看到虚拟内存增加了  
用文件来增大虚拟内存也是一样的道理

**范例1： 显示分区信息。**
~~~
[root@hnlinux ~]# sfdisk -l //显示分区信息

Disk /dev/sda: 1305 cylinders, 255 heads, 63 sectors/[tr](http://www.linuxso.com/command/tr.html)ack  
Units = cylinders of 8225280 bytes, blocks of 1024 bytes, counting from 0

  Device Boot Start   End  #cy[ls](http://www.linuxso.com/command/ls.html)  #blocks  Id System  
/dev/sda1  *   0+   12   13-  104391  83 **Linux**  
/dev/sda2     13  1304  1292  10377990  8e Linux LVM  
/dev/sda3     0    -    0     0  0 Empty  
/dev/sda4     0    -    0     0  0 Empty

Disk /dev/sdb: 652 cylinders, 255 heads, 63 sectors/track

[sfdisk](http://www.linuxso.com/command/sfdisk.html): ERROR: sector 0 does not have an msdos signature  
/dev/sdb: unrecognized partition  
No partitions found  
[root@hnlinux ~]#
~~~
  
**范例2： 关闭交换分区。**
~~~
[root@hnlinux ~]# swapoff /dev/sda2 // 关闭交换分区  
[root@hnlinux ~]#
~~~

## 三、利用swapoff和swapon刷新swap缓存

有时运行大量的进程后swap大量占用，达到30%的话机器会变得很慢  
**清除刷新swap**
可以用以下两个命令清除刷新swap  
~~~
swapoff -a  
swapon -a  
~~~  
这样swap就还原到初始状态  

**设置swap优先级**
以下是设置swap优先级的方法  
  
swappiness  
Ubuntu Feisty默认的vm.swappiness值是60,这一默认值已经很合适了。但你可以改小一些降低swap的加载，系统性能会有一点点的提升  
输 入：  
 
~~~
sysctl -q vm.swappiness
~~~
  
你会看到值是60， 更改:  
 

[su](http://www.linuxso.com/command/su.html)do sysctl vm.swappiness=10

  
这 样你就将值由60改为10,这可以大大降低系统对于swap的写入，建议内存为512m或更多的朋友采用此方法。如你你发现你对于swap的使用极少，可 以将值设为0。这并不会禁止你对swap的使用，而是使你的系统对于swap的写入尽可能的少，同时尽可能多的使用你的实际内存。这对于你在切换应用程序 时有着巨大的作用，因为这样的话它们是在物理内存而非swap分区中。  
如果你想永久得改变这一值，你需要更改 sysctl.conf 文件：  
 
~~~
vim /etc/sysctl.conf
~~~
  
添加：  
`vm.swappiness=10`
到末行，需要重启生效。  
我发现对 于我的1G内存，将此值设为5是最合适的。

**实例**

关闭所有的交换分区
~~~
[root@localhost ~]#  swapoff  – a         // 关闭所有交换分区 

[root@localhost ~]#  free                // 查看内存使用状态 

             total       used       free     shared   buffers     cached

Mem:       1659316     678908     980408          0      85608     369308

-/+ buffers/cache:     223992    1435324

Swap:            0          0          0          //swap 分区不使用
~~~


分类: [Linux命令](https://www.cnblogs.com/machangwei-8/category/1164784.html)
