Projects->build&Run->...->Run
E:\svn\PrintDll\test\PrintTest\uPrintTest\uPrintTest\bin\x64\Release\uPrintTest.exe
【source\main.cpp】
[dllexport:showDialog]
win ：指向C#创建的窗体中创建的Widget
mMainWidget ：指向主Widget：win中创建的QHBoxLayout Widget
vMainLayout_Main：指向一个QGridLayout
gMainTabPage：指向一个QTabWidget，该Widget被添加到mMainWidget中
最后初始化菜单mInitMenu->f_Init()，根据mButtomJsonStr添加到obj（类型：QJsonObject） 中
【source\Authority\testmenu.cpp】
定义了mButtomJsonStr变量，内容如下：
~~~xml
[{
		"key": "btn_Print",		//打印
		"parent": "Main",
		"val": "1",
		"class": "UPrint",
		"desc": "Print function "
	},
	{
		"key": "btn_SelectPixels",	//
		"parent": "Main",
		"val": "1",
		"class": "uSelectPixels",
		"desc": "Select Pixels "
	},
	{
		"key": "btn_PrintDelete",
		"parent": "Main",
		"val": "1",
		"class": "uPrintDelete",
		"desc": "Print Delete "
	},
	{
		"key": "btn_PrintAdd",
		"parent": "Main",
		"val": "1",
		"class": "uPrintAdd",
		"desc": "Print Add "
	},
	{
		"key": "btn_OpenSequence",
		"parent": "Main",
		"val": "1",
		"class": "uSysOpen",
		"desc": "Open Sequence "
	}
]
~~~