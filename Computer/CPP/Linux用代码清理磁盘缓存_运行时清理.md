**Linux用代码清理磁盘缓存（运行时清理磁盘缓存）**
# 基础：命令行清除缓存

## 清理缓存的命令行命令

```shell
sudo sh -c 'sync && echo 3 > /proc/sys/vm/drop_caches'
```

解释一下命令含义：使用同步向文件/proc/sys/vm/drop\_caches，写入3。

```shell
# 其中使用sh -c而不是直接使用
sudo sync && sudo echo 3 > /proc/sys/vm/drop_caches
# 是因为sudo echo 3 > /proc/sys/vm/drop_caches, 只是让echo 3有了sudo权限，但是 > 命令没有，而sh -c 它可以让 bash 将一个字串作为完整的命令来执行 
```

## 原理

/proc 是一个虚拟文件系统，我们可以通过对它的读写操作作为与kernel实体间进行通信的一种手段。也就是说可以通过修改/proc中的文件，来对当前kernel的行为做出调整。也就是说我们可以通过调整/proc/sys/vm/drop\_caches来释放[内存](https://so.csdn.net/so/search?q=%E5%86%85%E5%AD%98&spm=1001.2101.3001.7020)

```shell
echo 1 > /proc/sys/vm/drop_caches
echo 2 > /proc/sys/vm/drop_caches
echo 3 > /proc/sys/vm/drop_caches

# echo 0 是不释放缓存
# To free pagecache, use echo 1 > /proc/sys/vm/drop_caches; -- echo 1 是释放页缓存
# to freedentriesandinodes, use echo 2 > /proc/sys/vm/drop_caches; -- ehco 2 是释放dentries和inodes缓存
# to free pagecache, dentries and inodes, use echo 3 >/proc/sys/vm/drop_caches. -- echo 3 是释放 1 和 2 中说道的的所有缓存

# Because this is a non-destructive operation and dirty objects are not freeable, the user should run sync first. -- 由于..., 用户需要先运行sync
```

# 以代码释放磁盘缓存（即在程序运行过程中释放）

参照命令行命令，好像只需要在想要释放缓存的时候，首先sync，然后向文件/proc/sys/vm/drop\_caches写入3即可（记得使用sudo 运行程序）。（其实没有那么简单，感兴趣的可以看一下具体历程：“拓展：权限问题 + 代码优化”章节；只想要个结果可以不看🙈）。

## 代码如下

```cpp
sync();

int fd = open("/proc/sys/vm/drop_caches", O_WRONLY);
if (fd == -1) {
   printf("Cannot open file: %s \n", "/proc/sys/vm/drop_caches");
   printf("error: %s", strerror(errno));
   exit(0);
}
char *char_data_3 = "3";
write(fd, char_data_3, sizeof(char));
close(fd);
```

## 拓展：权限问题 + 代码优化

如果读取/proc/sys/vm/drop\_caches文件成功可以跳过本节。

原本使用的代码其实是：

```cpp
sync();
FILE *fp;
if (!(fp = fopen("/proc/sys/vm/drop_caches", "r"))) {
  printf("Cannot open file: %s \n", "/proc/sys/vm/drop_caches");
  printf("error: %s", strerror(errno));
  exit(0);
}
char* char_data_3 = "3";
fwrite(a, sizeof(int), 1, fp);
fclose(fp);
```

由于权限设置问题（即使使用sudo运行也不行），会报错

```shell
Cannot open file: /proc/sys/vm/drop_caches 
```

后面我尝试修改/proc/sys/vm/drop\_caches 的文件权限，结果失败（permission denied）

```shell
sudo chmod 777 /proc/sys/vm/drop_caches
```

又尝试使用lsattr和chattr命令，均以失败告终。

```shell
sudo su root
lsattr /proc/sys/vm/drop_caches
# lsattr: Permission denied While reading flags on /proc/sys/vm/drop_caches
```

### （权限问题）分析原因

因为文件其实是可以有比root更高的权限的，网上说可以使用lsattr查看，chattr修改，但是我并没有成功，原因我没有继续深究。

### （权限问题）解决办法

最终尝试使用更加原始的打开文件的方法（open比fopen更加底层），成功解决（即之前给出的代码👇）。

```cpp
sync();

int fd = open("/proc/sys/vm/drop_caches", O_WRONLY);
if (fd == -1) {
   printf("Cannot open file: %s \n", "/proc/sys/vm/drop_caches");
   exit(0);
}
char *char_data_3 = "3";
write(fd, char_data_3, sizeof(char));
close(fd);
```

当然，还是同样使用sudo权限运行哈。

### 最终代码（代码优化）使用fsync代替sync

```cpp
// sync();
int fd = open("/proc/sys/vm/drop_caches", O_WRONLY);
if (fd == -1) {
    printf("Cannot open file: %s \n", "/proc/sys/vm/drop_caches");
    printf("error: %s", strerror(errno));
   	exit(0);
}
char *char_data_3 = "3";
write(fd, char_data_3, sizeof(char));
// substitude sync
fsync(fd);
close(fd);
```

# 备注

本文是承接上一篇博客[direct io](https://blog.csdn.net/ahundredmile/article/details/124640814?spm=1001.2014.3001.5502) – 本文效果要更好一些（具体可以看direct io的“后记”）。