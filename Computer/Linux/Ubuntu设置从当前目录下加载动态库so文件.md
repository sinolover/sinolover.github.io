# Ubuntu设置从当前目录下加载动态库so文件

linux的excutable在执行的时候缺省是先搜索/lib和/usr/lib这两个目录，然后按照ld.so.conf里面的配置搜索绝对路径，linux缺省是不会在当前目录搜索动态库的。
windows加载动态库的时候，缺省是首先加载本地目录下的动态库，然后再搜索windows/system和windows/system32目录。
> windows的动态库搜索顺序，虽然有可能会造成潜在的混乱，但是对于开发和测试无疑是比较方便的，尤其是debug和release版本的动态库需要经常切换进行测试的时候。linux的动态库搜索顺序虽然可以说成是比较严谨，但是相对来说也比较呆板，有时候会造成不便。

ldd LB //查看进程依赖的动态库

其实，linux也可以支持“加载当前目录的动态库”。只要设置合适的环境变量LD\_LIBRARY\_PATH就可以了。设置方法有以下三种：

1、临时修改，log out之后就失效 
在terminal中执行：
~~~ cpp
export LD_LIBRARY_PATH=./
~~~
2、让当前帐号以后都优先加载当前目录的动态库
在Red Hat中修改~/.bash\_profile在文件末尾加上两行：
~~~ cpp
LD_LIBRARY_PATH=./
export LD_LIBRARY_PATH
~~~
（而在ubuntu中要修改的文件的名称是~/.bash_profile）

3、让所有帐号从此都优先加载当前目录的动态库**Ubuntu18.04实测有效**
修改/etc/profile在文件末尾加上两行： 
~~~ cpp
LD_LIBRARY_PATH=./
export LD_LIBRARY_PATH
~~~
PS：修改ld.so.conf不能达到我们的目的，因为ld.so.conf只支持绝对路径。