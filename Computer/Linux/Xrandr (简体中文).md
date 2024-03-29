转自：[Xrandr (简体中文) - ArchWiki](https://wiki.archlinux.org/title/Xrandr_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29#%E6%B7%BB%E5%8A%A0%E6%9C%AA%E8%A2%AB%E6%A3%80%E6%B5%8B%E5%88%B0%E7%9A%84%E6%9C%89%E6%95%88%E5%88%86%E8%BE%A8%E7%8E%87 "Xrandr (简体中文) - ArchWiki")

"xrandr" 是一款官方的 [RandR](https://en.wikipedia.org/wiki/RandR "RandR") (*Resize and Rotate*)[维基百科：X 窗口系统](https://en.wikipedia.org/wiki/X_Window_System "维基百科：X 窗口系统") 扩展配置工具。它可以设置屏幕显示的大小、方向、镜像等。对多显示器的情况，请参考 [Multihead](https://wiki.archlinux.org/title/Multihead "Multihead") 页面。

## 安装

[安装](https://wiki.archlinux.org/title/%E5%AE%89%E8%A3%85 "安装") 软件包 [xorg-xrandr](https://archlinux.org/packages/?name=xorg-xrandr "xorg-xrandr")。

### 图形化操作程序

*   **ARandR** — 提供了一个简单的图形化程序给 XrandR。显示器的位置会图形化地展示出来并可以使用拖拽的方式调整。

[https://christian.amsuess.com/tools/arandr/](https://christian.amsuess.com/tools/arandr/ "https://christian.amsuess.com/tools/arandr/") || [arandr](https://archlinux.org/packages/?name=arandr "arandr")

*   **LXRandR** — 是 LXDE 中的屏幕分辨率和外置显示器管理的工具.

[https://wiki.lxde.org/en/LXRandR](https://wiki.lxde.org/en/LXRandR "https://wiki.lxde.org/en/LXRandR") || GTK 2: [lxrandr](https://archlinux.org/packages/?name=lxrandr "lxrandr"), GTK 3: [lxrandr-gtk3](https://archlinux.org/packages/?name=lxrandr-gtk3 "lxrandr-gtk3")

### 命令行前端

*   **xlayoutdisplay** — 检测和排列显示器。可处理：笔记本电脑开合状态，最高有效刷新率，计算并设定当前的 DPI。最适合用在.xinitrc 中，以便于在接入或断开外置显示器或闭合笔记本盖子时引用这些参数

[GitHub - alex-courtis/xlayoutdisplay: Detects and arranges linux display outputs, using XRandR for detection and xrandr for arrangement.](https://github.com/alex-courtis/xlayoutdisplay "GitHub - alex-courtis/xlayoutdisplay: Detects and arranges linux display outputs, using XRandR for detection and xrandr for arrangement.") || [xlayoutdisplay](https://aur.archlinux.org/packages/xlayoutdisplay/ "xlayoutdisplay")AUR

## 测试配置

当没有添加任何选项直接运行时，*xrandr* 列出该系统可用的显示输出设备 (`VGA-1`, `HDMI-1` 等等) 和每一台设备可设置的分辨率，当前分辨率后面带有一个**\***号和一个**+**号:

xrandr

Screen 0: minimum 320 x 200, current 3200 x 1080, maximum 8192 x 8192
VGA-1 disconnected (normal left inverted right x axis y axis)
HDMI-1 connected primary 1920x1080+0+0 (normal left inverted right x axis y axis) 531mm x 299mm
   1920x1080     59.93 +  60.00\*   50.00    59.94  
   1920x1080i    60.00    50.00    59.94  
   1680x1050     59.88  
…

**注意：** 如果你的分辨率没有出现在上方， 请看 [#添加未被检测到的有效分辨率](https://wiki.archlinux.org/title/Xrandr_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29#%E6%B7%BB%E5%8A%A0%E6%9C%AA%E8%A2%AB%E6%A3%80%E6%B5%8B%E5%88%B0%E7%9A%84%E6%9C%89%E6%95%88%E5%88%86%E8%BE%A8%E7%8E%87 "#添加未被检测到的有效分辨率")

你可以使用 *xrandr* 设置不同的分辨率（必须是出现在上面输出列表中的分辨率）：

$ xrandr --output HDMI-1 --mode 1920x1080

当列表中出现多个刷新率，可以通过 `--rate` 选项改变，一次性设置或者分开设置，例如：

$ xrandr --output HDMI-1 --mode 1920x1080 --rate 60

如果输出设备已经连接但被禁用，`--auto` 选项会以系统偏好的分辨率（最大分辨率）开启特定的输出设备：

$ xrandr --output HDMI-1 --auto

还可以用一条命令设置多个输出设备，例如，使用系统偏好的选项关闭 `HDMI-1` 并打开 `HDMI-2`：

$ xrandr --output HDMI-1 --off --output HDMI-2 --auto

**注意：**

*   通过 *xrandr* 作出的改变只在本次会话中有效
*   *xrandr* 有很多功能 - 详情请参考 [xrandr(1)](https://man.archlinux.org/man/xrandr.1 "xrandr(1)")。

## 添加未被检测到的有效分辨率

由于出错的硬件或驱动，xrandr 可能并不能检测出您的显示器所有的有效分辨率。不过，我们可以在xrandr里添加所需要的分辨率。 Also, this same procedure can be used to add refresh rates you know are supported, but not enabled by your driver。

首先，运行`gtf`或者`cvt`，查询某分辨率的有效扫描频率。

 $ cvt 1280 1024
 
 # 1280x1024 59.89 Hz (CVT 1.31M4) hsync: 63.67 kHz; pclk: 109.00 MHz
 Modeline "1280x1024\_60.00"  109.00  1280 1368 1496 1712  1024 1027 1034 1063 -hsync +vsync

然后通过--newmode参数新建一种xrandr模式，输入上面所得到的查询结果，其中Modeline关键词自然需要被省略。

   xrandr --newmode "1280x1024\_60.00"  109.00  1280 1368 1496 1712  1024 1027 1034 1063 -hsync +vsync

新建模式后，我们需要把这模式添加到当前的输出设备（假定为VGA1）上。由于一些参数已经事先设置，只需输入模式名称即可，即1280x1024\_60.00。

   xrandr --addmode VGA1 1280x1024\_60.00

最后，再把VGA1的分辨率指定为刚刚添加的新模式。

   xrandr --output VGA1 --mode 1280x1024\_60.00

注意，以上设置同样地只能在当前会话暂时生效。

如果您对所要添加的某分辨率感到不放心，您可以追加新命令“sleep 5”以及一条切换到已有有效分辨率的命令，以保证不会被困在实际无效的分辨率，示例：

   xrandr --output VGA1 --mode 1280x1024\_60.00 && sleep 5 && xrandr --newmode "1024x768-safe" 65.00 1024 1048 1184 1344 768 771 777 806 -HSync -VSync && xrandr --addmode VGA1 1024x768-safe && xrandr --output VGA1 --mode 1024x768-safe

其他输出设备如法炮制：VGA1或DVI-I……

## 使xrandr所更改的分辨率设置永久生效

使xrandr定制永久生效的方案有：

*   `xorg.conf`（推荐）
*   `.xprofile`
*   kdm/gdm

### 在xorg.conf设置分辨率（推荐）

示例：

/etc/X11/xorg.conf

Section "Monitor"
    Identifier      "External DVI"
    Modeline        "1280x1024\_60.00"  108.88  1280 1360 1496 1712  1024 1025 1028 1060  -HSync +Vsync
    Option          "PreferredMode" "1280x1024\_60.00"
EndSection
Section "Device"
    Identifier      "ATI Technologies, Inc. M22 \[Radeon Mobility M300\]"
    Driver          "ati"
    Option          "Monitor-DVI-0" "External DVI"
EndSection
Section "Screen"
    Identifier      "Primary Screen"
    Device          "ATI Technologies, Inc. M22 \[Radeon Mobility M300\]"
    DefaultDepth    24
    SubSection "Display"
        Depth           24
        Modes   "1280x1024" "1024x768" "640x480"
    EndSubSection
EndSection

Section "ServerLayout"
        Identifier      "Default Layout"
        Screen          "Primary Screen"
EndSection

关于更多的配置细节，请阅读[Xorg (简体中文)](https://wiki.archlinux.org/title/Xorg_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29 "Xorg (简体中文)")或[xorg.conf(5)](https://man.archlinux.org/man/xorg.conf.5 "xorg.conf(5)")。

### 在xprofile设定xrandr命令

请阅读[xprofile](https://wiki.archlinux.org/title/Xprofile "xprofile").

这方案具有缺点：如果您使用[Display manager (简体中文)](https://wiki.archlinux.org/title/Display_manager_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29 "Display manager (简体中文)")的话，那么在启动进程之后很大程度上就会执行失败，最终无法顺利修改分辨率。

### 在KDM/GDM的启动脚本设定xrandr命令

KDM和GDM都具备在X初始化时，会被自动执行的启动脚本。GDM的启动脚本放在`/etc/gdm/`， KDM的则是`/usr/share/config/kdm/Xsetup`，SDDM 的则是在 `/usr/share/sddm/scripts/Xsetup`。您可以把相关的xrandr命令添加到这些启动脚本里。

这些脚本需要root权限及其他系统配置的配合，不过在启动进程里会比xprofile更早生效。

## 脚本

控制第二显示器的开关状态，默认显示器保持开启：
~~~bash
~/bin/xdisplay

#!/bin/bash
#
# This script toggles the extended monitor outputs if something is connected
#

# all available outputs
OUTPUTS=$(xrandr |awk '$2 ~ /connected/ {print $1}')

# your notebook LVDS monitor
DEFAULT\_OUTPUT=$(sed -ne 's/.\*(LVDS\[^ \]\*).\*/1/p' <<<$OUTPUTS)

# get info from xrandr
XRANDR=\`xrandr\`

EXECUTE=""

for CURRENT in $OUTPUTS
do
        if \[\[ $XRANDR == \*$CURRENT\\ connected\*  \]\] # is connected
        then
                if \[\[ $XRANDR == \*$CURRENT\\ connected\\ \\(\* \]\] # is disabled
                then
                        EXECUTE+="--output $CURRENT --auto --above $DEFAULT\_OUTPUT "
                else
                        EXECUTE+="--output $CURRENT --off "
                fi
        else # make sure disconnected outputs are off 
                EXECUTE+="--output $CURRENT --off "
        fi
done

xrandr --output $DEFAULT\_OUTPUT --auto $EXECUTE
~~~
在显示器之间切换，且只开启其中一个。
~~~bash
/usr/local/bin/toggle-display

#!/bin/bash
#
# toggle-display.sh
#
# Iterates through connected monitors in xrander and switched to the next one
# each time it is run.
#

# get info from xrandr
xStatus=\`xrandr\`
connectedOutputs=$(echo "$xStatus" | grep " connected" | sed -e "s/\\(\[A-Z0-9\]\\+\\) connected.\*/\\1/")
activeOutput=$(echo "$xStatus" | grep -e " connected \[^(\]" | sed -e "s/\\(\[A-Z0-9\]\\+\\) connected.\*/\\1/") 
connected=$(echo $connectedOutputs | wc -w)

# initialize variables
execute="xrandr "
default="xrandr "
i=1
switch=0

for display in $connectedOutputs
do

	# build default configuration
	if \[ $i -eq 1 \]
	then
		default=$default"--output $display --auto "
	else
		default=$default"--output $display --off "
	fi

	# build "switching" configuration
	if \[ $switch -eq 1 \]
	then
		execute=$execute"--output $display --auto "
		switch=0
	else
		execute=$execute"--output $display --off "
	fi

	# check whether the next output should be switched on
	if \[ $display = $activeOutput \]
	then
		switch=1
	fi

	i=$(( $i + 1 ))

done

# check if the default setup needs to be executed then run it
echo "Resulting Configuration:"
if \[ -z "$(echo $execute | grep "auto")" \]
then
	echo "Command: $default"
	\`$default\`
else
	echo "Command: $execute"
	\`$execute\`
fi
echo -e "\\n$(xrandr)"
~~~
您也可以使用xrr-events (from [AUR](https://github.com/aur-archive/xrr-events-git "AUR"))，一种负责监听XrandR事件的daemon服务。当某台显示器接通状态发生变动时，就会执行相关脚本。可在man页面进一步查询具体信息。

## 在VNC上使用xrandr

如果您在使用某台支持xrandr的VNC服务器，您可以通过"xrandr -s <width>x<height>"命令实时修改VNC的分辨率。tigervnc就是一种支持xrandr的VNC客户端。

示例：

`xrandr -s 1920x1200`

登陆VNC之后，如果您在控制台上输入"xrandr"，您将得到列出当前已配置模式的清单。每个模式均可通过xrandr -s选项激活。不过，若您所需要的模式并不在清单中，您可以按照以下来添加它。

示例：不妨想添加的是1024x600（上网本的一种常见分辨率）

首先执行CVT，得到理想分辨率所对应的正确刷新频率。

`$ cvt 1024 600`

您会得到如下类似的输出：
~~~bash
# 1024x600 59.85 Hz (CVT) hsync: 37.35 kHz; pclk: 49.00 MHz
Modeline "1024x600_60.00"   49.00  1024 1072 1168 1312  600 603 613 624 -hsync +vsync
~~~
在以下命令使用那些关于刷新频率的输出部分。
~~~bash
xrandr --newmode "1024x600"   49.00  1024 1072 1168 1312  600 603 613 624 -hsync +vsync
xrandr --addmode default "1024x600"
~~~
通过以上流程，输入xrandr -s 1024x600，就可以设置当前分辨率为1024x600，但是这设置只在当前的X会话暂时生效。为确保其模式永久可用，在`~/.vnc/xstartup`添加以下：
~~~bash
xrandr --newmode "1024x600"   49.00  1024 1072 1168 1312  600 603 613 624 -hsync +vsync
xrandr --addmode default "1024x600"r
~~~
## 疑难排除

### 添加未检测到的分辨率

由于硬件以及驱动程序可能的缺陷，例如，请求的EDID数据块不正确，导致 xrandr 可能不能准确侦测到显示器的分辨率。不过我们可以手工添加期望的分辨率。

首先，运行 `gtf` 或 `cvt` 以获取所需分辨率的 **模式行（Modeline）** ：
~~~bash
$ cvt 1280 1024

# 1280x1024 59.89 Hz (CVT 1.31M4) hsync: 63.67 kHz; pclk: 109.00 MHz
Modeline "1280x1024_60.00"  109.00  1280 1368 1496 1712  1024 1027 1034 1063 -hsync +vsync
~~~
**提示：** For some LCD screens (e.g. Samsung 2343NW, Acer XB280HK), the command `cvt -r` (= with reduced blanking) is to be used.

**注意：** 如果使用了 Intel 的显示驱动程序 [xf86-video-intel](https://archlinux.org/packages/?name=xf86-video-intel "xf86-video-intel")，期望的分辨率会在 `/var/log/Xorg.0.log` 中与其他特征值一并报告——如果该值不同于 `gtf` or `cvt` 的输出应首选该值。这里给出一个log文件的报告值与 xrandr 使用值的实例：
~~~bash
[    45.063] (II) intel(0): clock: 241.5 MHz   Image Size:  597 x 336 mm
[    45.063] (II) intel(0): h_active: 2560  h_sync: 2600  h_sync_end 2632 h_blank_end 2720 h_border: 0
[    45.063] (II) intel(0): v_active: 1440  v_sync: 1443  v_sync_end 1448 v_blanking: 1481 v_border: 0
~~~
xrandr --newmode "2560x1440" 241.50 2560 2600 2632 2720 1440 1443 1448 1481 -hsync +vsync

然后创建一个新的 xandr 模式。注意模式行中那些被忽略的关键字。

`$ xrandr --newmode "1280x1024_60.00"  109.00  1280 1368 1496 1712  1024 1027 1034 1063 -hsync +vsync`

创建完毕还需要一个操作步骤，把这个新模式追加到当前显示输出端口（VGA1）。我们只需引用模式的名称，因为参数已经在前面设置好了。

`$ xrandr --addmode VGA1 1280x1024_60.00`

现在，我们可以把屏幕分辨率切换为刚刚追加的值：

`$ xrandr --output VGA1 --mode 1280x1024_60.00`

注意，上述这些设置仅在本次会话中有效。

如果你不能确保将要测试的分辨率可用，可以在后面附加一个延迟和一个安全分辨率命令行，就像这样：
~~~bash
$ xrandr --output VGA1 --mode 1280x1024_60.00 && sleep 5 && xrandr --newmode "1024x768-safe" 65.00 1024 1048 1184 1344 768 771 777 806 -HSync -VSync && xrandr --addmode VGA1 1024x768-safe && xrandr --output VGA1 --mode 1024x768-safe
~~~
还有，要把 `VGA1` 改为正确的输出端口名。

EDID 校验和无效

如果前述方法导致引导期间发生 `*ERROR* EDID checksum is invalid` 错误，参阅 [这里](https://wiki.archlinux.org/title/Kernel_mode_setting_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29#Forcing_modes_and_EDID "这里") 和 [这里](https://askubuntu.com/questions/201081/how-can-i-make-linux-behave-better-when-edid-is-unavailable "这里").

也许 `xrandr --addmode` 会返回错误 `X Error of failed request: BadMatch`。NVIDIA 用户请参阅 [NVIDIA/Troubleshooting#xrandr BadMatch](https://wiki.archlinux.org/title/NVIDIA/Troubleshooting#xrandr_BadMatch "NVIDIA/Troubleshooting#xrandr BadMatch")。`BadMatch` 能指示出无效的 EDID 校验和。要验证确实是这种情况，请以 verbose mode 运行 X 服务（例如：`startx -- -logverbose 6`）然后查阅 Xorg 日志中有关 EDID 错误的信息。

### 纠正电视机分辨率过扫

With a flat panel TV, [w:overscan](https://en.wikipedia.org/wiki/overscan "w:overscan") looks like the picture is "zoomed in" so the edges are cut off.

Check your TV if there is a parameter to change. If not, apply an `underscan` and change border values. The required `underscan vborder` and `underscan hborder` values can be different for you, just check it and change it by more or less.

`$ xrandr --output HDMI-0 --set underscan on --set "underscan vborder" 25 --set "underscan hborder" 40`

### Correction of overscan tv resolutions via --transform

If underscan is not available another solution is using `xrandr --transform *a,b,c,d,e,f,g,h,i*`, which applies a transformation matrix on the output. See the [xrandr(1) § RandR\_version\_1.3\_options](https://man.archlinux.org/man/xrandr.1#RandR_version_1.3_options "xrandr(1) § RandR_version_1.3_options") manual page for the explanation of the transformation.

For example, the transformation scaling horizontal coordinates by `0.8`, vertical coordinates by `1.04` and moving the screen by 35 pixels right and 19 pixels down, is:

`$ xrandr --output HDMI1 --transform 0.80,0,-35,0,1.04,-19,0,0,1`

### Full RGB in HDMI

It may occur that the [Intel](https://wiki.archlinux.org/title/Intel "Intel") driver will not configure correctly the output of the HDMI monitor. It will set a limited color range (16-235) using the [Broadcast RGB property](https://patchwork.kernel.org/patch/1972181/ "Broadcast RGB property"), and the black will not look black, it will be grey.

To see if it is your case:

$ xrandr --output HDMI1 --set "Broadcast RGB" "Full"

Screen resolution reverts back after a blink

If you use [GNOME](https://wiki.archlinux.org/title/GNOME "GNOME") and your monitor does not have an EDID, above [#添加未检测到的分辨率](https://wiki.archlinux.org/title/Xrandr_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29#%E6%B7%BB%E5%8A%A0%E6%9C%AA%E6%A3%80%E6%B5%8B%E5%88%B0%E7%9A%84%E5%88%86%E8%BE%A8%E7%8E%87 "#添加未检测到的分辨率") might not work, with your screen just blinking once, after `xrandr --output`.

Poke around with `~/.config/monitors.xml`, or delete the file completely, and then reboot.

It is better explained in [this](https://unix.stackexchange.com/questions/184941/gnome-prevents-high-resolution-vga-without-edid-info-over-vga "this") article.

## 参见

*   [DualScreen](https://wiki.archlinux.org/title/DualScreen "DualScreen") Arch wiki page. How to get dual screens with Xrandr
*   [X/Config/Resolution - Ubuntu Wiki](https://wiki.ubuntu.com/X/Config/Resolution "X/Config/Resolution - Ubuntu Wiki")
*   [Debian Wiki - RandR 1.2 tutorial](https://wiki.debian.org/XStrikeForce/HowToRandR12 "Debian Wiki - RandR 1.2 tutorial")
*   [How I got Dual Monitors with Nouveau Driver / Applications & Desktop Environments / Arch Linux Forums](https://bbs.archlinux.org/viewtopic.php?pid=652861 "How I got Dual Monitors with Nouveau Driver / Applications & Desktop Environments / Arch Linux Forums")
*   [Randr12](https://nouveau.freedesktop.org/Randr12.html "Randr12")
*   [XStrikeForce/HowToRandR12 - Debian Wiki](https://wiki.debian.org/XStrikeForce/HowToRandR12 "XStrikeForce/HowToRandR12 - Debian Wiki")
*   man xrandr

[Categories](https://wiki.archlinux.org/title/Special:Categories "Categories"): 

*   [X server (简体中文)](https://wiki.archlinux.org/title/Category:X_server_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29 "X server (简体中文)")
*   [Xorg commands (简体中文)](https://wiki.archlinux.org/title/Category:Xorg_commands_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29 "Xorg commands (简体中文)")