
在嵌入式系统开发中，经常需要从主机上传送映像、文件等到目标机上。实现的方法有很多。如tftp，scp等。  
TFTP（Trivial File Transfer Protocol）是用来下载远程文件的最简单的网络协议，它基于UDP协议而实现。

# 一、TFTP的建立

嵌入式linux的tftp开发环境包括两个方面：一是linux服务器端的tftp-server支持，二是嵌入式目标系统的tftp-client支持。因为u-boot本身内置支持tftp-client，所以嵌入式目标系统端就不用配置了。我们要做的是在服务器端（即主机）上安装TFTP服务，并且正确地配置TFTP服务的路径和参数。  
首先需要安装：tftp-hpa  
```bash
sudoapt-get install tftp-hpa  
sudoapt-get install tftpd-hpa  
```
tftp-hpa是客户端，作用是从别人的TFTP服务器端上传/下载东西。  
tftpd-hpa是服务端，字母d代表daemon，作用是为别人提供TFTP服务，供别人上传/下载东西。

# 2、创建TFTP目录

首先需要建立一个TFTP目录，以供上传和下载。当然也可以使用现有的目录。然后需要设定该目录的权限，决定是否能够下载和上传文件。对于日常使用，我们一般就将其权限设置为最高，为所有用户组都添加所有权限（读+写+执行=4+2+1=7）：  
sudomkdir ~/tftp_boot  
sudochmod 777 tftp_boot –R  
我们的TFTP目录为/home/ghostar/tftp_boot，其权限已经是最高。

# 3、修改配置文件

修改tftpd-hpa相应的配置文件  
sudogedit /etc/default/tftpd-hpa  
原始的内容如下：
```bash

# /etc/default/tftpd-hpa

TFTP_USERNAME="tftp"  
TFTP_DIRECTORY="/var/lib/tftpboot"  
TFTP_ADDRESS="\[...\]:69"  
TFTP_OPTIONS="--secure"  
我将其修改为：

# /etc/default/tftpd-hpa

TFTP_USERNAME="ghostar"  
TFTP_DIRECTORY="/home/ghostar/tftp_boot"  
TFTP_ADDRESS="0.0.0.0:69"  
TFTP_OPTIONS="-l-c -s"  
说明：  
TFTP_USERNAME：必须改为当前的用户名，或者root；  
TFTP_DIRECTORY：我们设定的TFTP根目录；  
TFTP_OPTIONS：TFTP启动参数。意义如下：  
-l：以standalone/listen模式启动TFTP服务，而不是从inetd启动。  
（这里也表明，前面装的xinetd，其实是多此一举。其实，使用-l参数后，不需要再安装xinetd了）  
-c：可创建新文件。默认情况下，TFTP只允许覆盖原有文件，不能创建新文件。  
-s：改变TFTP启动的根目录。加了-s后，客户端使用TFTP时，不再需要输入指定目录，填写文件的完整路径，而是使用配置文件中写好的目录。这样也可以增加安全性。
```

我一开始没有注意TFTP_USERNAME这一项，随便取了一个名字，一直没有成功，后来改用自己的用户名，才测试成功。

# 4、重新启动服务

重启tftpd-hpa服务：  
sudo service tftpd-hpa restart  
如果显示如下，说明配置正确：  
tftpd-hpastart/running, process 2290  
之前我没有把TFTP_USERNAME该为用户名，而是随便取了一个，则会提示如下：  
tftpd-hpastart/running  
对比发现，这里并没有启动进程，因为配置中TFTP_USERNAME不正确，也就没有成功开启TFTP

# 5、确认tftp服务是否已经开启

查看tftp相关进程可以用以下指令：  
psaux |grep tftp  
弹出以下信息  
ghostar@ubuntu:~$ ps aux|grep tftp  
root 3151 0.0 0.0 15128 152 ? Ss 23:19 0:00 /usr/sbin/in.tftpd --listen --user ghostar --address 0.0.0.0:69 -l -c -s /home/ghostar/tftp_boot  
ghostar 3156 0.0 0.0 15956 956 pts/12 S+ 23:20 0:00 grep --color=auto tftp  
可以看到， /usr/sbin/in.tftpd已经启动，说明TFTP服务已经开启了，进程号正是3151。  
--listen对应配置文件中的参数 -l  
--user ghostar 就是配置文件中的TFTP_USERNAME  
/home/ghostar/tftp_boot是配置文件中的TFTP_DIRECTORY

另一种方法：  
netstat-a|grep tftp  
如果看到如下提示，说明TFTP服务开启了。  
udp 0 0 \*:tftp *:*

# 二、TFTP的使用

1、连接本机  
连接本机有三种方法，一是输入真实的IP地址，可以用ifconfig查得；二是用localhost来代表本机；三是使用地址127.0.0.1，这个IP地址始终代表本机的IP。

先在TFTP目录下新建一个文件a，在里面随便写一些内容，然后修改其权限为777。接着，输入以下指令的任意一条，进入TFTP命令行。  
tftp 192.168.1.201 （自己设定的IP）  
tftp localhost  
tftp127.0.0.1  
TFTP命令行的基本指令：  
put：将文件上传到TFTP目录  
get：取得TFTP目录上的文件  
quit/q：退出TFTP

因为TFTP服务将某一设定的目录视为根目录，因此不需要打出完整的路径。既然该目录下已经有一个文件a，我们就下面输入指令：  
tftp>get a  
tftp>put a  
如果没有任何提示，则说明传输成功。

下面看看当配置参数和文件权限改变时，会出现什么现象。我列举了一些常见问题：  
tftp>get a  
Transfer timed out.  
原因：tftpd服务没有启动。

需要注意的是，必须使TFTP的用户名和当前的系统的用户名一致，否则就无法成功启动tftpd服务。  
tftp>get a  
permission denied  
原因：操作者权限不够，比如当前的目录是/etc，不能随便get文件下来。需要提升权限。切换到root账户，或者直接执行sudo tftp。

tftp>put t1  
tftp: t1: No such file or directory  
原因：当前目录下没有t1文件

tftp>get d  
Error code 1: File not found  
原因：TFTP根目录下没有该文件

Error code 2: Only absolute filenamesallowed  
原因：TFTP启动配置参数没有-s，或者在DIRECTORY中没有填写目录

tftp>put b  
Error code 1: File not found  
原因：启动配置参数无-c，根目录下无同名文件  
（注意和前面情况的区别，不是当前目录下没有b文件，而是TFTP目录下找不到同名文件b）

tftp>put b  
Error code 2: File must have global writepermissions  
原因：根目录下有同名文件，该文件无写权限（启动配置参数有无-c都这样）

经测试，在tftp-hpa方法下，下列情况可以put成功：  
l 启动配置参数无-c，根目录下有同名文件，有写权限  
l 启动配置参数有-c，根目录下无同名文件  
l 启动配置参数有-c，根目录下有同名文件，有写权限

2、连接实验箱(未完成)

```bash
     实验箱操作系统中的TFTP服务已经装好，是在BusyBox v1.12.0中的。它的用法与本机的略有区别，但原理是一样的。
```

基本参数：  
-g： get，获取文件  
-p： put，长传文件  
-l FILE：本地的文件，名为FILE  
-r FILE：远程的文件，名为FILE  
实验箱的IP地址为192.168.1.200，我主机的IP地址为192.168.1.201。两者的IP应该在一个网段内，才能顺利通信。

使用举例：  
sudo minicom  
进入了实验箱的Linux操作系统。

```bash
  #cd /home
  #tftp -g 192.168.1.201 -r./hello -l./hello
  将主机TFTP目录下的文件hello下载到实验箱的当前目录（/home）。
  #tftp -p 192.168.1.201 -r./led -l./led
  将实验箱的当前目录（/home）的文件led上传到主机TFTP目录下。
```
