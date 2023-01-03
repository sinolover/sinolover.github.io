​

 转自：https://blog.csdn.net/lwbeyond/article/details/8431927

Ubuntu下为APT设置代理  
  
**一.最简单的方法**  
图形界面方法：新立得软件包管理器-->设置-->首选项-->网络。 进行设置代理就可以了。  
  
**二.编辑命令**
方法1：<font color=red>验证通过 </font>
如果您 **希望apt-get（而不是其他应用程序）一直使用http代理**，您可以使用这种方式。  
到/etc/apt/文件夹下的 <font color=red>**apt.conf**</font>文件, 没有的话就新建立一个配置文件。  
sudo gedit /etc/apt/apt.conf在您的apt.conf文件中加入下面这行。

```
Acquire::http::Proxy “http://proxyusr:password@yourproxyaddress:proxyport”;
Acquire::ftp::proxy "ftp://192.168.0.1:88/";
Acquire::https::proxy "https://192.168.0.1:88/";
```

(1) 根据你的实际情况替换 用户名:密码@地址和端口号。  
(2) 注意符号不要少写（不要丢了最后的一个分号）。  
  
**方法2：**  
如果您 **希望apt-get和其他应用程序如wget等都使用http代理**，您可以使用这种方式。  
这种方法会在您的主目录下的 <font color=red>**.bashrc**</font>文件中添加两行。  
gedit ~/.bashrc在您的.bashrc文件末尾添加如下内容。

```
export http_proxy=http://proxyusr:password@yourproxyaddress:proxyport
```

保存文件, 关闭当前终端，然后打开另一个终端。

如果您为了纠正错误而再次修改了配置文件，记得关闭终端并重新打开，否自新的设置不会生效。

<font color=red>**注意:如果之前执行过“系统－首选项－网络代理”的操作，需要注销才能使得新配置生效。**</font>

​