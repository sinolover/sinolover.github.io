# Eclipse
Eclipse Neon (4.6)
Eclipse Mars (2015-v4.5)
Eclipse Luna (2014-2015-v4.4)
Eclipse Kepler (2013-2014-v4.3)
Eclipse Juno (2012-2013-v4.2)
Eclipse Indigo (2011 - v3.7)
Eclipse Helios (2010-v3.6)
Eclipse Galileo (2009-v3.5)
Eclipse Ganymede(2008 - v 3.4)
Eclipse Europa Packages (2007 - v 3.3)


下面是目前已知的版本代号 （Release）【2014年10月】

Eclipse 3.1 版本代号 IO 【木卫1，伊奥】 

Eclipse 3.2 版本代号 Callisto 【木卫四，卡里斯托 】

Eclipse 3.3 版本代号 Eruopa 【木卫二，欧罗巴 】  

Eclipse 3.4 版本代号 Ganymede 【木卫三，盖尼米德 】

Eclipse 3.5 版本代号 Galileo 【伽利略】 

Eclipse 3.6 版本代号 Helios 【太阳神】

Eclipse 3.7 版本代号 Indigo 【靛青】

Eclipse 4.2 版本代号 Juno 

Eclipse 4.3 版本代号 Kepler 

Eclipse 4.4 版本代号 Luna 


13511222675 湖州 王龙 应该是博辕客户
畅想

wst
the web standard tools subproject
https://www.eclipse.org/webtools/wst/main.php


jst
j2ee Standard Tools Project
https://www.eclipse.org/webtools/jst/main.php

重构
（一般重构的快捷键都是Alt+Shift开头的了）
Alt+Shift+R 重命名方法名、属性或者变量名 （是我自己最爱用的一个了,尤其是变量和类的Rename,比手工方法能节省很多劳动力）
Alt+Shift+M 把一段函数内的代码抽取成方法 （这是重构里面最常用的方法之一了,尤其是对一大堆泥团代码有用）
Alt+Shift+C 修改函数结构（比较实用,有N个函数调用了这个方法,修改一次搞定）
Alt+Shift+L 抽取本地变量（ 可以直接把一些魔法数字和字符串抽取成一个变量,尤其是多处调用的时候）
Alt+Shift+F 把Class中的local变量变为field变量 （比较实用的功能）
Alt+Shift+I 合并变量（可能这样说有点不妥Inline）
Alt+Shift+V 移动函数和变量（不怎么常用）
Alt+Shift+Z 重构的后悔药（Undo）
Alt+Shift+R 选择好 关键字 按下这个按键 输入新的 关键字，实现全部一次行改名

Ctrl+Shift+O，这里O代表Organize Import的意思。自动补充import Package 。
Ctrl+Shift+F，这里面我们可以记忆F为Format格式化的意思。格式化代码缩进 。






跟编码有关的几个位置
1.有时候不小心copy来的单个文件编码与你workspace的默认编码不一致，就导致了单个乱码。解决办法：在Pakcage Explorer或者Project Explorer视图里面，右键点击该文件–>选择“Properties”–>”Text file encoding”–>给”Other”项设置相应的编码。
2.第三方jar包的编码问题可能是最常见的问题，其解决方案与单个文件的比较类似，在Pakcage Explorer或者Project Explorer视图里面，右键第三方jar包–>选择“Properties”–>给”Encoding”项设置相应的编码
3.有时候为了统一所有工程的编码，可能需要提前设置好各类型文件的字符编码，比如：把所有的jar类都设置为GBK编码，把所有的xml设置为UTF-8编码等。这时候就需要有针对性的给不同类型的文件设置不同的编码。步骤是这样的：Windows–>Perferences–>General–>Content Types–>选择文件类型–>设置“Default encoding”项
4.往往workspace都有一个默认的字符编码，如果你的工程大部分或者全部是另一个编码，那么你可以重新设置一个默认的字符编码，后续新建工程就不需要再去设置编码了。步骤是这样的：Windows–>Perferences–>General–>Workspace–>Test file encoding–>给“Other:”设置相应的编码


