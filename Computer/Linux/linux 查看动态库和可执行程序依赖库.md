​

转自：[linux 查看动态库和可执行程序依赖库\_帅的没朋友~的博客-CSDN博客\_linux 查看动态库](https://blog.csdn.net/song240948380/article/details/124226816 "linux 查看动态库和可执行程序依赖库_帅的没朋友~的博客-CSDN博客_linux 查看动态库")


# 一:objdump

```
# 查看依赖的库
objdump -x xxx.so | grep NEEDED

# 查看可执行程序依赖的库
objdump -x ./testTime | grep NEEDED

```

# 二:readelf

```
# 查看依赖的库
readelf -a xxx.so | grep "Shared"

# 查看可执行程序依赖的库
readelf -a ./testTime | grep "Shared"

# 查看依赖的库
readelf -d xxx.so
readelf -d ./testTime

# 查看静态库有哪些.o文件
readelf -d xxx.a

```

# 三:ldd

```
# 查看依赖的库
ldd xxx.so

# 查看可执行程序依赖的库
ldd ./testTime

```

# 四:进程是否依赖指定

```
lsof  ***.so
```
​