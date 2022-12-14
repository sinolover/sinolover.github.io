# 即看即用

**查看：**

> ethtool ethx          查看eth0网卡的基本设置
> 
> *内容包括网卡速率、网卡的工作模式等，其中 x 是对应网卡的编号，如eth0、eth1等等*
> 
> ethtool -i eth0      查看eth0网卡的驱动信息，内容包括驱动的型号、驱动的版本等
> 
> ethtool -d ethx      查询ethx网口注册性信息  
> ethtool -S ethx     查询ethx网口收发包统计 （注意S是大写）  
> ethtool -h              显示ethtool的命令帮助(help)  
> ethtool -r ethx      重置ethX网口到自适应模式
> 
> ethtool -g ethx     显示网卡的接收/发送环形参数。

**设置：**

注意：该命令只是临时设置，如果网卡重启就失效了，如果想要永久保存应该配置 /etc/sysconfig/network-scripts/ifcfg-ethx 文件，见后面详细信息。

> ethtool -s eth0 speed \[10|100|1000\]        # 设置网卡的速率，单位是：Mb/s
> 
> ethtool -s eth0 autoneg \[on|off\]                  # 设置网卡是否自动协商
> 
> 自动协商的内容主要包括工作模式、网卡速率以及流控等参数
> 
> ethtool -s ethX \[speed 10|100|1000\] \[duplex half|full\]  \[autoneg on|off\]        //设置网口速率10/100/1000M、设置网口半/全双工、设置网口是否自协商
> 
> ethtool -s eth0 autoneg off speed 100 duplex full # 也可以同时写在一起
> 
> ethtool -s eth0 duplex \[half|full\] #                   设置网卡的工作模式，可设置为半双工或全双工
> 
> ethtool -G eth0 rx 4096 /ethtool -G eth0 tx 4096                      //修改网卡的接收/发送环形参数。
> 
> ethtool -E eth0 magic 0x10798086 offset 0x10 value 0x1A   修改网卡EEPROM内容（0x1079 网卡device id , 0x8086网卡verdor id  ）
> 
> ethtool -e eth0  : dump网卡EEPROM内容

| <br> | <br> |
| --- | --- |
| ethX | 查询ethx网口基本设置，其中x是对应网卡的编号，如eth0、eth1等。 |
| \-k | 查询网卡的Offload信息。 |
| \-K | 修改网卡的Offload信息。 |
| \-c | 查询网卡聚合信息。 |
| \-C | 修改网卡聚合信息。 |
| \-l | 查看网卡队列数。 |
| \-L | 设置网卡队列数。 |

\-a 查看网卡中接收模块RX、发送模块TX和Autonegotiate模块的状态：启动on 或 停用off。

\-A 修改网卡中 接收模块RX、发送模块TX和Autonegotiate模块的状态：启动on 或 停用off。

\-c display the Coalesce(聚合、联合) information of the specified ethernet card.聚合网口信息，使看起来更有规律。

\-C Change the Coalesce setting of the specified ethernet card.修改网卡聚合信息。

\-g Display the rx/tx ring parameter information of the specified ethernet card. 显示网卡的接收/发送环形参数。

\-G Change the rx/tx ring setting of the specified ethernet card. 修改网卡的接收/发送环形参数。

\-i 显示网卡驱动的信息，如驱动的名称、版本等。

\-d 显示register dump信息, 部分网卡驱动不支持该选项。

\-e 显示EEPROM dump信息，部分网卡驱动不支持该选项。

\-E 修改网卡EEPROM byte.

\-k 显示网卡Offload参数的状态：on 或 off，包括rx-checksumming、tx-checksumming等。

\-K 修改网卡Offload参数的状态

\-p 用于区别不同ethX对应网卡的物理位置，常用的方法是使网卡port上的led不断的闪；N指示了网卡闪的持续时间，以秒为单位。

\-r 如果auto-negotiation模块的状态为on，则restarts auto-negotiation.

\-s 修改网卡的部分配置，包括网卡速度、单工/全双工模式、mac地址等。加上-s选项修改的内容才会生效

\-S 显示NIC- and driver-specific 的统计参数，如网卡接收/发送的字节数、接收/发送的广播包个数等。

\-t 让网卡执行自我检测，有两种模式：offline or online.

# 详细信息

## 软件简介

（[ethtool首页、文档和下载 - 网卡诊断和调整工具 - OSCHINA - 中文开源技术交流社区](https://www.oschina.net/p/ethtool?hmsr=aladdin1e1 "ethtool首页、文档和下载 - 网卡诊断和调整工具 - OSCHINA - 中文开源技术交流社区")）

ethtool是一个 Linux 下功能强大的网络管理工具，目前几乎所有的网卡驱动程序都有对 ethtool 的支持，可以用于网卡状态/驱动版本信息查询、收发数据信息查询及能力配置以及网卡工作模式/链路速度等查询配置。

它可以用来：

*   获取标识和诊断信息
*   获取扩展的设备统计信息
*   控制以太网设备的速度、双工、自动协商和流控制
*   控制校验和卸载及其他硬件卸载功能
*   控制 DMA 环大小及中断控制
*   控制多队列设备的接收队列选择
*   升级闪存中的固件

## 安装

[https://my.oschina.net/u/4342102/blog/4910174](https://my.oschina.net/u/4342102/blog/4910174 "https://my.oschina.net/u/4342102/blog/4910174")

## ethtool的使用

[https://my.oschina.net/u/4342102/blog/4910174](https://my.oschina.net/u/4342102/blog/4910174 "https://my.oschina.net/u/4342102/blog/4910174")

```bash
ethtool [ -a | -c | -g | -i | -d | -k | -r | -S |] ethX
ethtool [-A] ethX [autoneg on|off] [rx on|off] [tx on|off]
ethtool [-C] ethX [adaptive-rx on|off] [adaptive-tx on|off] [rx-usecs N] 
             [rx-frames N] [rx-usecs-irq N] [rx-frames-irq N] [tx-usecs N] 
             [tx-frames N] [tx-usecs-irq N] [tx-frames-irq N] [stats-block-usecs N]
             [pkt-rate-low N][rx-usecs-low N] [rx-frames-low N] [tx-usecs-low N] 
             [tx-frames-low N] [pkt-rate-high N] [rx-usecs-high N] [rx-frames-high N] 
             [tx-usecs-high N] [tx-frames-high N] [sample-interval N]
ethtool [-G] ethX [rx N] [rx-mini N] [rx-jumbo N] [tx N]
ethtool [-e] ethX [raw on|off] [offset N] [length N]
ethtool [-E] ethX [magic N] [offset N] [value N]
ethtool [-K] ethX [rx on|off] [tx on|off] [sg on|off] [tso on|off]
ethtool [-p] ethX [N]
ethtool [-t] ethX [offline|online]
ethtool [-s] ethX [speed 10|100|1000] [duplex half|full] [autoneg on|off] 
             [port tp|aui|bnc|mii] [phyad N] [xcvr internal|external]
[wol p|u|m|b|a|g|s|d...] [sopass xx:yy:zz:aa:bb:cc] [msglvl N]

```

> 参数说明:  
> \-a        //查看网卡中接收模块RX、发送模块TX和Autonegotiate模块的状态：启动on 或 停用off。  
> \-A        //修改网卡中 接收模块RX、发送模块TX和Autonegotiate模块的状态：启动on 或 停用off。  
> \-c        //display the Coalesce(聚合、联合) information of the specified ethernet card.聚合网口信息，使看起来更有规律。  
> \-C        //Change the Coalesce setting of the specified ethernet card.修改网卡聚合信息。  
> \-g        //Display the rx/tx ring parameter information of the specified ethernet card. 显示网卡的接收/发送环形参数。  
> \-G        //Change the rx/tx ring setting of the specified ethernet card. 修改网卡的接收/发送环形参数。  
> \-i        //显示网卡驱动的信息，如驱动的名称、版本等。  
> \-d        //显示register dump信息, 部分网卡驱动不支持该选项。  
> \-e        //显示EEPROM dump信息，部分网卡驱动不支持该选项。  
> \-E        //修改网卡EEPROM byte.  
> \-k        //显示网卡Offload参数的状态：on 或 off，包括rx-checksumming、tx-checksumming等。  
> \-K        //修改网卡Offload参数的状态  
> \-p        //用于区别不同ethX对应网卡的物理位置，常用的方法是使网卡port上的led不断的闪；N指示了网卡闪的持续时间，以秒为单位。  
> \-r        //如果auto-negotiation模块的状态为on，则restarts auto-negotiation.  
> \-s        //修改网卡的部分配置，包括网卡速度、单工/全双工模式、mac地址等。加上-s选项修改的内容才会生效  
> \-S        //显示NIC- and driver-specific 的统计参数，如网卡接收/发送的字节数、接收/发送的广播包个数等。  
> \-t        //让网卡执行自我检测，有两种模式：offline or online. 

## 输出详解

**ethtool eth0**

> \[root@localhost ~\]# ethtool eth0  
> Settings for eth0:  
> Supported ports: \[ TP \]  
>    
> // 支持模式  
> Supported link modes: 10baseT/Half 10baseT/Full  
> 100baseT/Half 100baseT/Full  
> 1000baseT/Full  
> Supported pause frame use: No  
> Supports auto-negotiation: Yes // 支持自动协商  
> Supported FEC modes: Not reported  
>    
> // 通告模式  
> Advertised link modes: 10baseT/Half 10baseT/Full  
> 100baseT/Half 100baseT/Full  
> 1000baseT/Full  
> Advertised pause frame use: No  
> Advertised auto-negotiation: Yes // 使用自动协商  
> Advertised FEC modes: Not reported  
>    
> Speed: 1000Mb/s // 当前速率 1000Mb/s  
> Duplex: Full // 工作模式为全双工  
>    
> Port: Twisted Pair  
> PHYAD: 0  
> Transceiver: internal  
>    
> Auto-negotiation: on // 自动协商打开  
>    
> MDI-X: off (auto)  
> Supports Wake-on: d  
> Wake-on: d  
> Current message level: 0x00000007 (7)  
> drv probe link  
> Link detected: yes

 **输出格式**：

> **\# ethtool** **\-k eth0**
> 
> Features for eth0: 
> rx-checksumming: on 
> tx-checksumming: on 
> scatter-gather: on 
> tcp-segmentation-offload: on
> 
> **\# ethtool -l eth0**
> 
> Channel parameters for eth0: 
> Pre-set maximums: 
> … 
> Current hardware settings: 
> … 
> Combined:       8 
> 
> **\# ethtool -c eth0**
> 
> Coalesce parameters for eth0: 
> Adaptive RX: off  TX: off 
> … 
> rx-usecs：30 
> rx-frames：50 
> … 
> tx-usecs：30 
> tx-frames：1

参数含义如下：

| 参数 | 说明 |
| --- | --- |
| rx-checksumming | 接收包校验和。 |
| tx-checksumming | 发送包校验和。 |
| scatter-gather | 分散-聚集功能，是网卡支持TSO的必要条件之一。 |
| tcp-segmentation-offload | 简称为TSO，利用网卡对TCP数据包分片。 |
| Combined | 网卡队列数。 |
| adaptive-rx | 接收队列的动态聚合执行开关。 |
| adaptive-tx | 发送队列的动态聚合执行开关。 |
| tx-usecs | 产生一个中断之前至少有一个数据包被发送之后的微秒数。 |
| tx-frames | 产生中断之前发送的数据包数量。 |
| rx-usecs | 产生一个中断之前至少有一个数据包被接收之后的微秒数。 |
| rx-frames | 产生中断之前接收的数据包数量。 |

[ethtool工具\_鲲鹏通用\_鲲鹏性能优化十板斧\_网络子系统性能调优\_常用性能监测工具\_华为云](https://support.huaweicloud.com/tuningtip-kunpenggrf/kunpengtuning_12_0022.html "ethtool工具_鲲鹏通用_鲲鹏性能优化十板斧_网络子系统性能调优_常用性能监测工具_华为云")

## **其他指令**

ethtool -A tx off eth0    停止网卡的发送模块TX

*操作完毕后，可输入：ethtool -a eth0，查看tx模块是否已被停止。*

ethtool -K eth0 rx off 关闭网卡对收到的数据包的校验功能

*操作完毕后，可输入：ethtool -k eth0，查看校验功能是否已被停止。*

ethtool -p eth0 10  如果机器上安装了两块网卡，查看eth0对应着哪块网卡

*操作完毕后，看哪块网卡的led灯在闪，eth0就对应着哪块网卡。*  
 

## **将 ethtool 设置永久保存**

>  将 ethtool 设置永久保存在网络设备有两种方法，一种是写入网口配置文件中，一种是开机自启动脚本。
> 
>  解决方法一:  
>  ethtool 设置可通过 /etc/sysconfig/network-scripts/ifcfg-ethX 文件保存，从而在设备下次启动时激活选项。   
> 例如：ethtool -s eth0 speed 100 duplex full autoneg off  
> 此指令将eth0设备设置为全双工自适应，速度为100Mbs。若要eth0启动时设置这些参数, 修改文件/etc/sysconfig/network-scripts/ifcfg-eth0 ，添加如下一行:   
> ETHTOOL\_OPTS="speed 100 duplex full autoneg off"
> 
> 解决方法二:  
>  将ethtool设置写入/etc/rc.d/rc.local之中。

#  如何使用 ethtool 优化 Linux 虚拟机网卡性能

在高并发应用场景下，Linux 虚拟机可能会出现丢包现象，可以通过调整网卡的 Buffer size 来缓解此问题。

本文使用 ethtool 来查看和修改 RX/TX 值来获取更好性能，以下为参考示例：

在 ethtool 配置文件中可以看到，RX/TX 值的单位是 section size：

shell复制

```
#define NETVSC_SEND_SECTION_SIZE                6144
#define NETVSC_RECV_SECTION_SIZE                1728
```

1.  查看当前网卡 RX/TX 参数：
    
    shell复制
    
    ```
    [root@centos75 ~]# ethtool -g eth0
    Ring parameters for eth0:
    Pre-set maximums:
    RX:                   18811
    RX Mini:              0
    RX Jumbo:             0
    TX:                   2560
    Current hardware settings:
    RX:                   10486
    RX Mini:              0
    RX Jumbo:             0
    TX:                   192
    ```
    
     重要
    
    Pre-set maximums 中的 RX/TX 值为该网卡的 Buffer size 最大值；  
    Current hardware settings 中 RX/TX 值代表该网卡当前的 Buffer size 大小。  
    所以，设置的 Current hardware settings 的 RX/TX 值必须在 Pre-set maximums 的限制之内。
    
2.  调整 RX/TX 参数：
    
    shell复制
    
    ```
    [root@centos75 ~]# ethtool -G eth0 rx 18000 tx 2500
    ```
    
3.  查看调整后的 RX/TX 参数：
    
    shell复制
    
    ```
    [root@centos75 ~]# ethtool -g eth0
    Ring parameters for eth0:
    Pre-set maximums:
    RX:                   18811
    RX Mini:              0
    RX Jumbo:             0
    TX:                   2560
    Current hardware settings:
    RX:                   18000
    RX Mini:              0
    RX Jumbo:             0
    TX:                   2500
    ```
    

由于重启之后修改会失效，您可以在 *rc.local* 中设置为开机自动运行修改命令，参考如下：

1.  添加可执行权限：
    
    shell复制
    
    ```
    [root@centos75 ~]# chmod -x /etc/rc.d/rc.local
    ```
    
2.  添加修改命令，并保存：
    
    shell复制
    
    ```
    [root@centos75 ~]# vi /etc/rc.d/rc.local
    
    #!/bin/bash
    # THIS FILE IS ADDED FOR COMPATIBILITY PURPOSES
    #
    # It is highly advisable to create own systemd services or udev rules
    # to run scripts during boot instead of using this file.
    #
    # In contrast to previous versions due to parallel execution during boot
    # this script will NOT be run after all other services.
    #
    # Please note that you must run 'chmod +x /etc/rc.d/rc.local' to ensure
    # that this script will be executed during boot.
    
    touch /var/lock/subsys/local
    ethtool -G eth0 rx 18000 tx 2500
    ```
    
3.  重启并验证：
    
    shell复制
    
    ```
    [root@centos75 ~]# ethtool -g eth0
    Ring parameters for eth0:
    Pre-set maximums:
    RX:                   18811
    RX Mini:              0
    RX Jumbo:             0
    TX:                   2560
    Current hardware settings:
    RX:                   18000
    RX Mini:              0
    RX Jumbo:             0
    TX:                   2500
    ```
    

[如何使用 ethtool 优化 Linux 虚拟机网卡性能 | Azure Docs](https://docs.azure.cn/zh-cn/articles/azure-operations-guide/virtual-machines/linux/aog-virtual-machines-linux-howto-optimize-performance-of-linux-nic-by-ethtool "如何使用 ethtool 优化 Linux 虚拟机网卡性能 | Azure Docs")

# ethtool 解决网卡丢包严重和网卡原理

（原文;[ethtool 解决网卡丢包严重和网卡原理\_一个人的旅行的博客-CSDN博客\_网卡丢包](https://blog.csdn.net/u011857683/article/details/83758869 "ethtool 解决网卡丢包严重和网卡原理_一个人的旅行的博客-CSDN博客_网卡丢包")）

1 概述  
 

最近业务上老有问题，查看发现overruns值不断增加，学习了一下相关的知识。发现数值也在不停的增加。发现这些 errors, dropped, overruns 表示的含义还不大一样。

\[root@localhost ~\]# ifconfig eth0  
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST> mtu 1500  
inet 192.168.1.135 netmask 255.255.255.0 broadcast 192.168.1.255  
inet6 fe80::20c:29ff:fe9b:52d3 prefixlen 64 scopeid 0x20<link>  
ether 00:0c:29:9b:52:d3 txqueuelen 1000 (Ethernet)  
RX packets 833 bytes 61846 (60.3 KiB)  
RX errors 0 dropped 0 overruns 0 frame 0  
TX packets 122 bytes 9028 (8.8 KiB)  
TX errors 0 dropped 0 overruns 0 carrier 0 collisions 0

(1) RX errors

表示总的收包的错误数量，这包括 too-long-frames 错误，Ring Buffer 溢出错误，crc 校验错误，帧同步错误，fifo overruns 以及 missed pkg 等等。

(2) RX dropped

表示数据包已经进入了 Ring Buffer，但是由于内存不够等系统原因，导致在拷贝到内存的过程中被丢弃。

(3) RX overruns

表示了 fifo 的 overruns，这是由于 Ring Buffer(aka Driver Queue) 传输的 IO 大于 kernel 能够处理的 IO 导致的，而 Ring Buffer 则是指在发起 IRQ 请求之前的那块 buffer。很明显，overruns 的增大意味着数据包没到 Ring Buffer 就被网卡物理层给丢弃了，而 CPU 无法即使的处理中断是造成 Ring Buffer 满的原因之一，上面那台有问题的机器就是因为 interruprs 分布的不均匀(都压在 core0)，没有做 affinity 而造成的丢包。

(4) RX frame

表示 misaligned 的 frames。

2 网卡工作原理  
 

2.1 网卡收包  
网线上的packet首先被网卡获取，网卡会检查packet的CRC校验，保证完整性，然后将packet头去掉，得到frame。网卡会检查MAC包内的目的MAC地址，如果和本网卡的MAC地址不一样则丢弃(混杂模式除外)。

网卡将frame拷贝到网卡内部的FIFO缓冲区，触发硬件中断。（如有ring buffer的网卡，好像frame可以先存在ring buffer里再触发软件中断（下篇文章将详细解释Linux中frame的走向），ring buffer是网卡和驱动程序共享，是设备里的内存，但是对操作系统是可见的，因为看到linux内核源码里网卡驱动程序是使用kcalloc来分配的空间，所以ring buffer一般都有上限，另外这个ring buffer size，表示的应该是能存储的frame的个数，而不是字节大小。另外有些系统的 ethtool 命令 并不能改变ring parameters来设置ring buffer的大小，暂时不知道为什么，可能是驱动不支持。）

网卡驱动程序通过硬中断处理函数，构建sk\_buff，把frame从网卡FIFO拷贝到内存skb中，接下来交给内核处理。（支持napi的网卡应该是直接放在ring buffer，不触发硬中断，直接使用软中断，拷贝ring buffer里的数据，直接输送给上层处理，每个网卡在一次软中断处理过程能处理weight个frame）

过程中，网卡芯片对frame进行了MAC过滤，以减小系统负荷。（除了混杂模式）

2.2 网卡发包  
网卡驱动程序将IP包添加14字节的MAC头，构成frame（暂无CRC）。Frame（暂无CRC）中含有发送端和接收端的MAC地址，由于是驱动程序创建MAC头，所以可以随便输入地址，也可以进行主机伪装。

驱动程序将frame（暂无CRC）拷贝到网卡芯片内部的缓冲区，由网卡处理。

网卡芯片将未完全完成的frame（缺CRC）再次封装为可以发送的packet，也就是添加头部同步信息和CRC校验，然后丢到网线上，就完成一个IP报的发送了，所有接到网线上的网卡都可以看到该packet。

2.3 网卡中断处理函数  
 

产生中断的每个设备都有一个相应的中断处理程序，是设备驱动程序的一部分。每个网卡都有一个中断处理程序，用于通知网卡该中断已经被接收了，以及把网卡缓冲区的数据包拷贝到内存中。

当网卡接收来自网络的数据包时，需要通知内核数据包到了。网卡立即发出中断。内核通过执行网卡已注册的中断处理函数来做出应答。中断处理程序开始执行，通知硬件，拷贝最新的网络数据包到内存，然后读取网卡更多的数据包。

这些都是重要、紧迫而又与硬件相关的工作。内核通常需要快速的拷贝网络数据包到系统内存，因为网卡上接收网络数据包的缓存大小固定，而且相比系统内存也要小得多。所以上述拷贝动作一旦被延迟，必然造成网卡FIFO缓存溢出 - 进入的数据包占满了网卡的缓存，后续的包只能被丢弃，这也应该就是ifconfig里的overrun的来源。

当网络数据包被拷贝到系统内存后，中断的任务算是完成了，这时它把控制权交还给被系统中断前运行的程序。

2.4 缓冲区访问  
网卡的内核缓冲区，是在PC内存中，由内核控制，而网卡会有FIFO缓冲区，或者ring buffer，这应该将两者区分开。FIFO比较小，里面有数据便会尽量将数据存在内核缓冲中。

网卡中的缓冲区既不属于内核空间，也不属于用户空间。它属于硬件缓冲，允许网卡与操作系统之间有个缓冲；

内核缓冲区在内核空间，在内存中，用于内核程序，做为读自或写往硬件的数据缓冲区；

用户缓冲区在用户空间，在内存中，用于用户程序，做为读自或写往硬件的数据缓冲区；

另外，为了加快数据的交互，可以将内核缓冲区映射到用户空间，这样，内核程序和用户程序就可以同时访问这一区间了。

对于有ring buffer的网卡，ring buffer是由驱动与网卡共享的，所以内核可以直接访问ring buffer，一般拷贝frames的副本到自己的内核空间进行处理（deliver到上层协议，之后的一个个skb就是按skb的指针传递方式传递，直到用户获得数据，所以，对于ring buffer网卡，大量拷贝发生在frame从ring buffer传递到内核控制的计算机内存里）。

3 丢包排查  
网卡工作在数据链路层，数据量链路层，会做一些校验，封装成帧。我们可以查看校验是否出错，确定传输是否存在问题。然后从软件层面，是否因为缓冲区太小丢包。

3.1 先查看硬件情况  
一台机器经常收到丢包的报警，先看看最底层的有没有问题:

(1) 查看工作模式是否正常

\[root@localhost ~\]# ethtool eth0 | egrep 'Speed|Duplex'  
Speed: 1000Mb/s  
Duplex: Full  
 

(2) 查看检验是否正常

\[root@localhost ~\]# ethtool -S eth0 | grep crc  
rx\_crc\_errors: 0  
   
Speed，Duplex，CRC 之类的都没问题，基本可以排除物理层面的干扰。

3.2 通过 ifconfig 可以看到 overruns 是否一直增大  
 

for i in \`seq 1 100\`; do ifconfig eth2 | grep RX | grep overruns; sleep 1; done  
 

这里一直增加

RX packets:346547657 errors:0 dropped:0 overruns:35345 frame:0  
 

3.3 查看buffer大小  
 

可以通过ethtool来修改网卡的buffer size ，首先要网卡支持，我的服务器是是INTEL 的1000M网卡,我们看看ethtool说明 

\-g   –show-ringQueries the specified ethernet device for rx/tx ring parameter information.  
   
\-G   –set-ringChanges the rx/tx ring parameters of the specified ethernet device.  
 

(1) 查看当前网卡的buffer size情况

ethtool -g eth0  
\[root@localhost ~\]# ethtool -g eth0  
Ring parameters for eth0:  
Pre-set maximums:  
RX: 4096  
RX Mini: 0  
RX Jumbo: 0  
TX: 4096  
Current hardware settings:  
RX: 256  
RX Mini: 0  
RX Jumbo: 0  
TX: 256  
 

3.4 修改buffer size大小  
ethtool -G eth0 rx 2048  
   
ethtool -G eth0 tx 2048  
 

\[root@localhost ~\]# ethtool -G eth0 rx 2048  
\[root@localhost ~\]#  
\[root@localhost ~\]#  
\[root@localhost ~\]# ethtool -G eth0 tx 2048  
\[root@localhost ~\]#  
\[root@localhost ~\]#  
\[root@localhost ~\]# ethtool -g eth0  
Ring parameters for eth0:  
Pre-set maximums:  
RX: 4096  
RX Mini: 0  
RX Jumbo: 0  
TX: 4096  
Current hardware settings:  
RX: 2048  
RX Mini: 0  
RX Jumbo: 0  
TX: 2048  
  
原文链接：https://blog.csdn.net/u011857683/article/details/83758869

**参考和摘抄：**

《ethtool 命令详解》[ethtool 命令详解\_一个人的旅行的博客-CSDN博客\_ethtool](https://blog.csdn.net/u011857683/article/details/83758689 "ethtool 命令详解_一个人的旅行的博客-CSDN博客_ethtool")

《[ethtool 网卡诊断和调整工具](https://www.oschina.net/p/ethtool "ethtool 网卡诊断和调整工具")》[ethtool首页、文档和下载 - 网卡诊断和调整工具 - OSCHINA - 中文开源技术交流社区](https://www.oschina.net/p/ethtool?hmsr=aladdin1e1 "ethtool首页、文档和下载 - 网卡诊断和调整工具 - OSCHINA - 中文开源技术交流社区")