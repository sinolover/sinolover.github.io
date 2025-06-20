# Linux中常用的命令都是哪些单词的缩写
说明：大写的字母就是缩写中对应的字母
首先最重要的：Linux -- LINUs' uniX (开个玩笑不是这样的，别当真）
然后是推动Linux发展和辉煌的GNU -- Gnu is Not Unix is Not Unix is Not Unix is Not Unix......(很魔性的起名方式，但我很喜欢）
## 目录名：
/boot：顾名思义
/root ：同上
/lost+found：同上
/run：同上
/home：同上
/etc：ETCetera
/bin：BINaries
/dev：DEVices
/lib：LIBraries
/mnt：MouNT
/proc：PROCesses
/tmp：TeMPorary
/var：VARiable
/srv：SeRVices
/opt：OPTion
/sbin：Super BINaries（又作Superuser BINaries）
/sys：SYStem
/usr：一鼓而作Unix System Resources，再而作Unix Software Resources,三而作Unix Shared Resources（这个很重要，很多人会认为这个是user）

## 系统管理

### 文件管理（包括文件输出管理）：
ls -- LiSt
cd -- Change Directory
pwd -- Print Working Directory
cp -- CoPy
mv -- MoVe
rm -- ReMove
pushd -- PUSH to Directory
popd -- POP from Directory（这俩个是很有用的命令，在编译LFS的时候学到的）
mkdir -- MaKe DIRectory
rmdir -- ReMove DIRectory
cat -- CATenate（有说是conCATenate，这两个词是一个意思吧）
sed -- Stream EDitor
diff -- DIFFerence
wc -- Word Count（不是那个wc啊）
chmod -- CHange MODe
chown -- CHange OWNer
chgrp -- CHange GRouP
awk -- Aho Weinberger and Kernighan（自恋，服气）(  @xx xxxx 谢谢指错）
gawk -- Gnu Aho Weinberger and Kernighan
grep -- General Regular Expression Print(  @闫子昂 谢谢）
ln -- LiNk
tar -- TARball

### 硬件管理
df -- Disk Free
du -- Disk Usage
dd -- Data Description（一说是Convert and Copy， 但是cc被用掉了，就用dd了）
parted -- PARTition EDitor
lspci -- LiSt Peripheral Component Interconnect
lscpu -- LiSt Central Process Unit
lsusb -- LiSt Universal Serial Bus
mdadm -- 求助啊，这个不知道啊，只知道md是multiple device！！
注：等我有时间会补上lvm的内容

### 软件及软件包管理
man -- MANual
apt -- Advanced Packaging Tool
dpkg -- Debian PacKaGe
yum -- Yellow dog Updater, Modified
rpm -- RPM Package Manager（又作Redhat Package Manager，不过我比较喜欢前者，因为魔性）

### 系统管理（主要是内核模组）：
depmod -- DEPend MODule
lsmod -- LiSt MODule
modprobe -- MODule PROBE
modinfo -- MODule INFOrmation
insmod -- INSert MODule
rmmod -- ReMove MODule
ps -- Processes Status
su -- Substitute User
bash -- Bourne Again SHell
init -- INITialization（差点把这个忘了）
ssh -- Secure SHell
wine -- Wine Is Not an Emulator（哈哈，又来了）
exec -- EXECute
fstab -- FileSystem TABle
passwd -- PASSWorD
tty -- TeleTYpe
sudo -- SuperUser DO
grub -- GRand Unified Bootloader（Linux全家桶启动就靠它了）
tzselect -- Time Zone SELECT
sync -- SYNChronize（确认关机前多执行几次。有次我在tty1用vim改nginx配置文件，朋友在tty2给我直接关机了（用halt关的，这个猪脑子，我说过好多次用shutdown了），导致开机的时候nginx启动不起来，关机前配置文件我改了很多，但是vim没有备份那么多，还要重改www）
装了systemd -- SYSTEM Daemon 的话，会有systemctl bootctl journalctl loginctl localectl timedatectl 等等，都是blablabla ConTroL

### 编辑器：
ed -- EDitor
nano -- Nano's ANOther editor（还有）
emacs -- Editor MACroS（还有很多全称，就不罗列了，这个接受度比较广）
vi -- VIsual
vim -- Vi Improved

### 用户及用户组管理

### 程序开发
cc -- C Compiler
gcc -- Gnu Compiler Collection（作为一个 软件集被你下载下来编译安装的时候）
gcc -- Gnu C Compiler (作为一个 软件被你调用来编译C程序的时候）
g++ -- Gnu c++ compiler（其实g++只是调用gcc，然后连接c++的库，并且作相应的一些编译设置而已）
gcj -- Gnu Compiler for Java
yacc -- Yet Another Compiler Compiler
guile -- Gnu Ubiquitous Intelligent Language for Extensions
gas -- Gnu Assembler（好像已经看不到了）
php -- PHP：Hypertext Preprocessor （啊啊啊又来了）
ld -- Link eDitor（为啥不是le我不太明白，但是GNU官方名叫Linker，但是没有d啊？？难道是Link Daemon？？不懂）（  @唐喵 谢提醒，我又去查了一遍）
下面引用他的评论：
关于 ld... 如果我是设计者的话，我会担心跟表示“小于等于”的 le 重名。也有材料说是来源于 load (see GNU linker).
gdb -- Gnu DeBug（又忘神器了）
tcl -- Tool Command Line

### 图形界面
gnome -- GNu Object Model Environment
gdm -- Gnome Display Manager
gtk -- Graphic user interface ToolKit
qt -- ………………Toolkit（不说Q了，用Q只是因为在开发者的Emacs中Q特别漂亮…………）
kde -- K Desktop Environment
lxde -- Lightweight X11 Desktop Environment
xfce -- XForms Common Environment

然后是各种参数（声明：只是通常会使用的参数，并不一定是通用参数，使用时请注意）：
-h | --help：help
-v | --version：version（吐槽一下java）

### 许可证：
gnu gpl : Gnu General Public License
gnu lgpl : Gnu General Lesser Public License
gfdl : Gnu Free Documentation License
agpl : Affero General Public License（或简写为Affero gpl)
apsl : Apple Public Source License
bsd : Berkeley Software Distribution license

g开头的命令大都是GNU的，反正GNU是一个很神奇的组织