1. ubuntu无法引导
>Then I found: No ACPI support for my PC, what can I do? from irrational_john ... way to go .. acpi=ht didn't work but pci=noacpi has done the trick. For your hardware I'd recommend John's approach pf cycling through the options he provided:

bash# cd /etc/default
bash# sudo vi grub

修改 ：GRUB_CMDLINE_LINUX_DEFAULT=“quiet splash pci=noacpi”

bash# sudo update-grub
bash# sudo reboot

2. arduino自己的代码感觉被人改过
最后经过我现场观察，发现是他在用鼠标的滚轮时，编辑器里执行了粘贴动作！后来叫他换了个好一点的鼠标就解决问题了。
网上找了一个解决方法，就是用Xmodmap彻底屏蔽中键：做法是
$ echo "pointer = 1 25 3 4 5 6 7 2" > ~/.Xmodmap
重新登录就可生效。
但是这么做的话，鼠标中键关闭浏览器标签的功能就不能用了。