# wscript.shell 
WshShell对象的ProgID

WScript.Shell是[WshShell](https://baike.baidu.com/item/WshShell/7339090?fromModule=lemma_inlink)对象的ProgID，创建WshShell对象可以运行程序、操作注册表、创建快捷方式、访问系统文件夹、管理环境变量。

外文名

wscript.shell

信    息

WshShell 对象

说    明

CurrentDirectory 返回

## 目录

1.  1 [基本信息](https://baike.baidu.com/item/wscript.shell#1)
2.  2 [属性 说明](https://baike.baidu.com/item/wscript.shell#2)
3.  3 [方法 说明](https://baike.baidu.com/item/wscript.shell#3)
4.  4 [详解](https://baike.baidu.com/item/wscript.shell#4)

1.  ▪ [Environment](https://baike.baidu.com/item/wscript.shell#4_1)
2.  ▪ [SpecialFolders](https://baike.baidu.com/item/wscript.shell#4_2)
3.  ▪ [CreateShortcut](https://baike.baidu.com/item/wscript.shell#4_3)
4.  ▪ [Popup](https://baike.baidu.com/item/wscript.shell#4_4)
5.  ▪ [RegDelete](https://baike.baidu.com/item/wscript.shell#4_5)

1.  ▪ [RegRead](https://baike.baidu.com/item/wscript.shell#4_6)
2.  ▪ [RegWrite](https://baike.baidu.com/item/wscript.shell#4_7)
3.  ▪ [Run](https://baike.baidu.com/item/wscript.shell#4_8)

## 基本信息

[编辑](javascript:;) [播报](javascript:;)

WshShell 对象

ProgID WScript.Shell

文件名 wshom.ocx

CLSID F935DC22-1CF0-11d0-ADB9-00C04FD58A0B

IID F935DC21-1CF0-11d0-ADB9-00C04FD58A0B

## 属性 说明

[编辑](javascript:;) [播报](javascript:;)

CurrentDirectory 返回或改变当前目录

Environment 返回 WshEnvironment 集合对象。

SpecialFolders 使用 WshSpecialFolders 对象提供对 Windows shell 文件夹的访问，如桌面文件夹，[开始菜单](https://baike.baidu.com/item/%E5%BC%80%E5%A7%8B%E8%8F%9C%E5%8D%95?fromModule=lemma_inlink)文件夹和个人文档文件夹。

## 方法 说明

[编辑](javascript:;) [播报](javascript:;)

| 方法 | 说明 |
| --- | --- |
| AppActivate | 激活一个应用程序窗口。 |
| CreateShortcut | 创建并返回 WshShortcut 对象。 |
| Exec | 在子命令窗口中运行一个应用程序，提供访问StdIn/StdOut/StdErr流。 |
| [ExpandEnvironmentStrings](https://baike.baidu.com/item/ExpandEnvironmentStrings?fromModule=lemma_inlink) | 扩展 PROCESS[环境变量](https://baike.baidu.com/item/%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F?fromModule=lemma_inlink)并返回结果字符串。 |
| LogEvent | 添加一个事件到日志文件。 |
| Popup | 显示包含指定消息的消息窗口。 |
| RegDelete | 从[注册表](https://baike.baidu.com/item/%E6%B3%A8%E5%86%8C%E8%A1%A8?fromModule=lemma_inlink)中删除指定的键或值。 |
| RegRead | 从注册表中返回指定的键或值。 |
| RegWrite | 在注册表中设置指定的键或值。 |
| Run | 创建新的进程，该进程用指定的窗口样式执行指定的命令。 |
| SendKeys | 发送一个或多个按键到活动窗口。 |

## 详解

[编辑](javascript:;) [播报](javascript:;)

### Environment

Environment 属性返回 WshEnvironment 对象。

语法

WshShell.Environment ( \[strType\]) = objWshEnvironment

注释

若 strType 指定了[环境变量](https://baike.baidu.com/item/%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F?fromModule=lemma_inlink)所处的位置，可能值为 "System"、"User"、"Volatile" 和 "Process"。若未提供 strType，则该方法在 Windows NT 中检索系统环境变量或在 Windows 95 中检索进程环境变量。

对于 Windows 95，strType 参数仅支持 "Process"。

下列变量是由 Windows 操作系统提供的。脚本也可获取由其他应用程序设置的环境变量。

名称 说明

NUMBER\_OF\_PROCESSORS 计算机上运行的处理器数目。

PROCESSOR\_ARCHITECTURE 用户[工作站](https://baike.baidu.com/item/%E5%B7%A5%E4%BD%9C%E7%AB%99?fromModule=lemma_inlink)使用的处理器类型。

PROCESSOR\_IDENTIFIER 用户工作站的处理器 ID。

PROCESSOR\_LEVEL 用户工作站的处理器级。

PROCESSOR\_REVISION 用户工作站的处理器版本。

OS 用户工作站所用的操作系统。

COMSPEC 用于运行“命令提示”窗口的命令（通常为 [cmd.exe](https://baike.baidu.com/item/cmd.exe?fromModule=lemma_inlink)）。

HOMEDRIVE 本地主[驱动器](https://baike.baidu.com/item/%E9%A9%B1%E5%8A%A8%E5%99%A8?fromModule=lemma_inlink)（通常为 C 驱动器）。

HOMEPATH 用户的默认路径（在 Windows NT 上通常为 usersdefault）。

PATH 路径环境变量。

PATHEXT [可执行文件](https://baike.baidu.com/item/%E5%8F%AF%E6%89%A7%E8%A1%8C%E6%96%87%E4%BB%B6?fromModule=lemma_inlink)的扩展名（通常为 .com、 .exe、.bat 或 .cmd）。

PROMPT [命令提示符](https://baike.baidu.com/item/%E5%91%BD%E4%BB%A4%E6%8F%90%E7%A4%BA%E7%AC%A6?fromModule=lemma_inlink)（通常为 $P$G）。

SYSTEMDRIVE 系统所在的本地驱动器（例如，c:）。

SYSTEMROOT 系统目录（例如，c:winnt）。和 WINDIR 相同。

WINDIR 系统目录（例如 c:winnt）。和 SYSTEMROOT 相同。

TEMP 存储[临时文件](https://baike.baidu.com/item/%E4%B8%B4%E6%97%B6%E6%96%87%E4%BB%B6?fromModule=lemma_inlink)的目录（例如，c:temp）。用户可更改。

TMP 存储临时文件的目录（例如，c:temp）。用户可更改。

示例

~~~java
'返回NUMBER_OF_PROCESSORS系统环境变量
Dim WshShell, WshSysEnv
Set WshShell = CreateObject("WScript.Shell")
Set WshSysEnv = WshShell.Environment("SYSTEM")
WScript.Echo WshSysEnv("NUMBER_OF_PROCESSORS")
~~~
**WshEnvironment 对象**

WshEnvironment 对象未直接给出，可用 WshShell.Environment 属性来访问。

下面描述与 WshEnvironment 对象关联的属性。

属性 说明

Item 获取或设置指定的[环境变量](https://baike.baidu.com/item/%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F?fromModule=lemma_inlink)值。

Count 枚举项的数目。

length 枚举项的数目 (JScript)。

下面描述与 WshEnvironment 对象关联的方法。

方法 说明

Remove 删除指定的环境变量。

### SpecialFolders

SpecialFolders 属性提供 WshSpecialFolders 对象以便访问 Windows 的 shell 文件夹，例如桌面文件夹、[开始菜单](https://baike.baidu.com/item/%E5%BC%80%E5%A7%8B%E8%8F%9C%E5%8D%95?fromModule=lemma_inlink)文件夹和个人文档文件夹。

语法

WshShell.SpecialFolders = objWshSpecialFolders

示例
~~~java
'这段代码展示如何访问桌面文件夹
Dim WshShell
Set WshShell = CreateObject("WScript.Shell")
MsgBox "Your desktop is " & WshShell.SpecialFolders("Desktop")
~~~

**WshSpecialFolders 对象**

该对象未直接给出。要得到 WshSpecialFolders 对象，请使用 WshShell.SpecialFolders 属性。

下面描述与 WshSpecialFolders 对象关联的属性。

属性 描述

Item 指定文件夹的完整路径（默认）。

Count 枚举项的数目。

length 枚举项的数目 (JScript) 。

WshSpecialFolders.Item

Item 属性返回由 strFolderName 指定的文件夹的完整路径。它是默认属性。

语法

WshShell.SpecialFolders.Item("strFolderName") = strFolderPath

WshShell.SpecialFolders("strFolderName") = strFolderPath

注释

若请求的文件夹 (strFolderName) 不可用，则 WshShell.SpecialFolders("strFolderName") 返回 NULL。例如，Windows 95 没有 AllUsersDesktop 文件夹，如果 strFolderName = AllUsersDesktop，则返回 NULL。

Windows 95 和 [Windows NT 4.0](https://baike.baidu.com/item/Windows%20NT%204.0?fromModule=lemma_inlink)操作系统提供下列指定文件夹：

AllUsersDesktop

AllUsersStartMenu

AllUsersPrograms

AllUsersStartup

Desktop

Favorites

Fonts

MyDocuments

NetHood

PrintHood

Programs

Recent

SendTo

StartMenu

Startup

Templates

示例
~~~java
Dim WshShell, StrMyDesktop
Set WshShell = CreateObject("WScript.Shell") '创建对象是wshell对象，不要和wscript对象混了
StrMyDesktop = WshShell.SpecialFolders("Desktop") '这段返回完整的Windows桌面文件夹路径，这段可以不要
For Each strFolder In WshShell.SpecialFolders '遍历所有特殊文件夹，这里的SpecialFolders是属性
    MsgBox strFolder '显示所有特殊文件夹
Next
~~~

### CreateShortcut

CreateShortcut 方法创建 WshShortcut 对象并将其返回。如果快捷方式标题以 .url 结尾，就会创建 WshURLShortcut 对象。

语法

WshShell.CreateShortcut(strPathname) = objShortcut

示例
~~~java
'这段代码创建一个指向当前执行脚本的快捷方式
Dim WshShell, oShellLink, oUrlLink
Set WshShell = CreateObject("WScript.Shell")
Set oShellLink = WshShell.CreateShortcut("CurrentScript.lnk")
oShellLink.TargetPath = Wscript.ScriptFullName
oShellLink.Save
Set oUrlLink = WshShell.CreateShortcut("MicrosoftWebSite.URL")
oUrlLink.TargetPath = "http://..." '输入网站 URL
oUrlLink.Save
~~~

**WshShortcut 对象**

该对象未直接给出。要获得 WshShortcut 对象，请使用 WshShell.CreateShortcut 方法。

下面说明和 WshShortcut 对象有关的属性。

属性 说明

Arguments 快捷方式对象的参数。

Description 快捷方式对象的说明。

Hotkey 快捷方式对象的[热键](https://baike.baidu.com/item/%E7%83%AD%E9%94%AE?fromModule=lemma_inlink)。

IconLocation 快捷方式对象的图标位置。

TargetPath 快捷方式对象的目标路径。

WindowStyle 快捷方式对象的窗口样式。

WorkingDirectory 快捷方式对象的工作目录。

下面说明与 WshShortcut 对象有关的方法。

方法 说明

Save 将快捷方式存储到指定的文件系统中。

WshShortcut.Arguments

Arguments 属性提供快捷方式对象的参数。

语法

WshShortcut.Arguments = strArguments

WshShortcut.Description

Description 属性提供快捷方式对象的说明。

语法

WshShortcut.Description = strDescription

WshShortcut.Hotkey

HotKey 属性提供快捷方式对象的[热键](https://baike.baidu.com/item/%E7%83%AD%E9%94%AE?fromModule=lemma_inlink)。热键是启动或切换程序的键盘快捷方式。

语法

WshShortcut.HotKey = strHotKey

注释

strHotKey 的BNF语法如下：

Hotkey ::= modifier\* keyname

modifier ::= "ALT+" | "CTRL+" | "SHIFT+" | "EXT+"

keyname ::= "A" .. "Z" |

"0".. "9" |

"Back" | "Tab" | "Clear" | "Return" |

"Escape" | "Space" | "Prior" | ...

所有键的名称都可以在 WINUSER.H 中找到。[热键](https://baike.baidu.com/item/%E7%83%AD%E9%94%AE?fromModule=lemma_inlink)不区分大小写。

热键只能激活位于 Windows 桌面或 Windows“开始”菜单的快捷方式。

Windows [资源管理器](https://baike.baidu.com/item/%E8%B5%84%E6%BA%90%E7%AE%A1%E7%90%86%E5%99%A8?fromModule=lemma_inlink)不接受 ESC、ENTER、TAB、SPACE、PRINT SCREEN 或 BACKSPACE，即使 WshShortcut.Hotkey 遵循 Win32 API 支持它们。因此，建议在快捷方式中不要用这些键。

示例
~~~java
Dim WshShell, strDesktop, oMyShortcut
Set WshShell = CreateObject("WScript.Shell")
strDesktop = WshShell.SpecialFolders("Desktop")
Set oMyShortcut = WshShell.CreateShortcut(strDesktop & "\a_key.lnk")
OMyShortcut.TargetPath = "%windir%\notepad.exe"
oMyShortCut.Hotkey = "ALT+CTRL+F"
oMyShortCut.Save
~~~

WshShortcut.IconLocation

IconLocation 属性提供快捷方式对象的图标位置。图标位置的格式应为 "Path,index"。

语法

WshShortcut.IconLocation = strIconLocation

WshShortcut.TargetPath

TargetPath 属性提供快捷方式对象的目标路径。

语法

WshShortcut.TargetPath = strTargetPath

WshShortcut.WindowStyle

WindowStyle 属性提供快捷方式对象的窗口样式。

语法

WshShortcut.WindowStyle = natWindowStyle

WshShortcut.WorkingDirectory

WorkingDirectory 为一个快捷方式对象提供工作目录。

语法

WshShortcut.WorkingDirectory = strWorkingDirectory

WshShortcut.Save

Save 方法把快捷方式对象保存到由 FullName 属性指定的位置。

语法

WshShortcut.Save

**WshUrlShortcut 对象**

该对象未直接给出。要获取 WshUrlShortcut 对象，可使用 WshShell.CreateShortcut 方法。

下表说明了和 WshUrlShortcut 对象有关的属性。

属性 说明

FullName URL 快捷方式对象的完整路径。

TargetPath URL 快捷方式对象的目标路径。

下表说明了和 WshUrlShortcut 对象有关的方法。

方法 说明

Save 将快捷方式保存到指定的文件系统中。

WshUrlShortcut.FullName

FullName 属性提供快捷方式对象的完整路径。

语法

WshUrlShortcut.FullName = strFullName

WshUrlShortcut.TargetPath

TargetPath 属性提供快捷方式对象的目标路径。

语法

WshUrlShortcut.TargetPath = strTargetPath

WshUrlShortcut.Save

Save 方法保存一个快捷方式，该快捷方式指向 FullName 属性指定的位置。

语法

WshUrlShortcut.Save

**ExpandEnvironmentStrings**

[ExpandEnvironmentStrings](https://baike.baidu.com/item/ExpandEnvironmentStrings?fromModule=lemma_inlink) 方法在 strString 中扩展 PROCESS 环境变量并返回结果字符串。变量被 "%" 字符括起。

[环境变量](https://baike.baidu.com/item/%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F?fromModule=lemma_inlink)不区分大小写。

语法

WshShell.ExpandEnvironmentStrings(strString) = strExpandedString

示例
~~~java
Dim WshShell
Set WshShell = CreateObject("WScript.Shell")
MsgBox "Prompt is " & WshShell.ExpandEnvironmentStrings("%PROMPT%")
~~~

### Popup

Popup 方法显示一个弹出式[消息框](https://baike.baidu.com/item/%E6%B6%88%E6%81%AF%E6%A1%86?fromModule=lemma_inlink)窗口，消息框中包含的消息由 strText 指定。该消息框的窗口标题由 strTitle 指定。若 strTitle 省略，则窗口标题为 Windows Scripting Host。

语法

WshShell.Popup(strText, \[natSecondsToWait\], \[strTitle\], \[natType\]) = intButton

注释

若提供 natSecondsToWait 且其值大于零，则消息框在 natSecondsToWait 秒后关闭。

natType 的含义与其在 Win32? MessageBox 函数中相同。下表显示 natType 中的值及含义。下表中的值可以组合。

按钮类型

| 值 | 说明 |
| --- | --- |
| 0 | 显示“确定”按钮 |
| 1 | 显示“确定”和“取消”按钮 |
| 2 | 显示“终止”、“重试”和“忽略”按钮 |
| 3 | 显示“是”、“否”和“取消”按钮 |
| 4 | 显示“是”和“否”按钮 |
| 5 | 显示“重试”和“取消”按钮 |

图标类型

| 值 | 说明 |
| --- | --- |
| 16 | 显示停止标记图标 |
| 32 | 显示问号图标 |
| 48 | 显示感叹号图标 |
| 64 | 显示信息标记图标 |

以上两个表并不涵盖 natType 的所有值。完整的列表请参阅 Win32 文档。

返回值 intButton 指示用户所单击的按扭编号。若用户在 natSecondsToWait 秒之前不单击按扭，则 intButton 设置为 -1 。

| 值 | 说明 |
| --- | --- |
| 1 | “确定”按钮 |
| 2 | “取消”按钮 |
| 3 | “终止”按钮 |
| 4 | “重试”按钮 |
| 5 | “忽略”按钮 |
| 6 | “是”按钮 |
| 7 | “否”按钮 |

示例
~~~java
Dim WshShell
Set WshShell = CreateObject("WScript.Shell")
WshShell.Popup "Where do you want to go today?"
~~~

WScript.Echo

Echo 方法在窗口（Wscript.exe 中）或“[命令提示符](https://baike.baidu.com/item/%E5%91%BD%E4%BB%A4%E6%8F%90%E7%A4%BA%E7%AC%A6?fromModule=lemma_inlink)”窗口（cscript.exe 中）显示参数。

参数用空格分隔。在 cscript.exe 中，该方法在显示最后一个参数之后输出一对回车/换行（CR LF）。

语法

Wscript.Echo \[anyArg...\]

示例
~~~java
WScript.Echo
WScript.Echo 1,2,3
WScript.Echo "Windows Scripting Host is cool."
~~~

### RegDelete

RegDelete 从[注册表](https://baike.baidu.com/item/%E6%B3%A8%E5%86%8C%E8%A1%A8?fromModule=lemma_inlink)中删除名为 strName 的键或值。

语法

WshShell.RegDelete strName

参数

strName

如果 strName 以[反斜杠](https://baike.baidu.com/item/%E5%8F%8D%E6%96%9C%E6%9D%A0?fromModule=lemma_inlink) () 结束，则该方法[删除键](https://baike.baidu.com/item/%E5%88%A0%E9%99%A4%E9%94%AE?fromModule=lemma_inlink)而不是值。

strName 参数必须以下列之一的根键名开始：

短根键名 长根键名

HKCU HKEY\_CURRENT\_USER

HKLM HKEY\_LOCAL\_MACHINE

HKCR HKEY\_CLASSES\_ROOT

HKEY\_USERS

HKEY\_CURRENT\_CONFIG

示例
~~~java
Dim WshShell
Set WshShell = CreateObject("WScript.Shell")
WshShell.RegDelete "HKCU\ScriptEngine\Value" '删除值"Value"
WshShell.RegDelete "HKCU\ScriptEngine\Key" '删除键"Key"
~~~

### RegRead

RegRead 方法返回名为 strName 的[注册表](https://baike.baidu.com/item/%E6%B3%A8%E5%86%8C%E8%A1%A8?fromModule=lemma_inlink)键或值。

语法

WshShell.RegRead(strName) = strValue

参数

strName

如果 strName 以反斜杠 () 结束，则该方法返回键，而不是值。

strName 参数必须以下列根键名开始。

Short Long

HKCU HKEY\_CURRENT\_USER

HKLM HKEY\_LOCAL\_MACHINE

HKCR HKEY\_CLASSES\_ROOT

HKEY\_USERS

HKEY\_CURRENT\_CONFIG

注释

RegRead 方法仅支持 REG\_SZ、REG\_EXPAND\_SZ、REG\_DWORD、REG\_BINARY 和 REG\_MULTI\_SZ 数据类型。若[注册表](https://baike.baidu.com/item/%E6%B3%A8%E5%86%8C%E8%A1%A8?fromModule=lemma_inlink)有其他数据类型，RegRead 返回 DISP\_E\_TYPEMISMATCH。

示例
~~~java
Dim WshShell
Set WshShell = CreateObject("WScript.Shell")
WshShell.RegRead("HKCU\ScriptEngine\Val") 'Read from value "Val"
WshShell.RegRead("HKCU\ScriptEngine\Key") 'Read from key "Key"
~~~

### RegWrite

RegWrite 方法设置名为 strName 的[注册表](https://baike.baidu.com/item/%E6%B3%A8%E5%86%8C%E8%A1%A8?fromModule=lemma_inlink)键或值。

语法

WshShell.RegWrite strName, anyValue, \[strType\]

参数

strName

若 strName 以一个反斜杠 () 结束，则该方法设置键，而不是值。

strName 参数必须以下列根键名开头。

Short Long

HKCU HKEY\_CURRENT\_USER

HKLM HKEY\_LOCAL\_MACHINE

HKCR HKEY\_CLASSES\_ROOT

HKEY\_USERS

HKEY\_CURRENT\_CONFIG

anyValue

当 strType 为 REG\_SZ 或 REG\_EXPAND\_SZ 时，RegWrite 方法自动将 anyValue 转换为字符串。若 strType 为 REG\_DWORD，则 anyValue 被转换为整数。若 strType 为 REG\_BINARY，则 anyValue 必须是一个整数。

strType

RegWrite 方法支持 strType 为 REG\_SZ、REG\_EXPAND\_SZ、REG\_DWORD 和 REG\_BINARY。若其他的数据类型被作为 strType 传递，RegWrite 返回 E\_INVALIDARG。

示例
~~~java
Dim WshShell
Set WshShell = CreateObject("WScript.Shell")
WshShell.RegWrite "HKCU\ScriptEngine\Value", "Some string value"
WshShell.RegWrite "HKCU\ScriptEngine\Key", 1, "REG_DWORD"
~~~

### Run

Run 方法创建一个新的进程，该进程以 intWindowStyle 窗口样式执行 strCommand。

语法

WshShell.Run (strCommand, \[intWindowStyle\], \[blnWaitOnReturn\])

参数

strCommand

在 strCommand 参数内部的[环境变量](https://baike.baidu.com/item/%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F?fromModule=lemma_inlink)被自动扩展。

intWindowStyle

这是为新进程在 STARTUPINFO 结构内设置的 wShowWindow 元素的值。其意义与 ShowWindow 中的 nCmdShow 参数相同，可取以下值之一。

| 名称 | 值 | 含义 |
| --- | --- | --- |
| SW\_HIDE | 0 | 隐藏窗口并激活另一窗口。 |
| SW\_SHOWNORMAL | 1 | 激活并显示一个窗口。若窗口是最小化或最大化，则恢复到其原来的大小和位置。 |
| SW\_SHOWMINIMIZED | 2 | 激活窗口并以最小化显示该窗口。 |
| SW\_SHOWMAXIMIZED | 3 | 激活窗口并以最大化显示该窗口。 |
| SW\_SHOWNOACTIVATE | 4 | 按窗口最近的大小和位置显示。活动窗口保持活动。 |
| SW\_SHOW | 5 | 以当前大小和位置激活并显示窗口。 |
| SW\_MINIMIZE | 6 | 最小化指定窗口并激活按 Z 序排序的下一个顶层窗口。 |
| SW\_SHOWMINNOACTIVE | 7 | 最小化显示窗口。[活动窗口](https://baike.baidu.com/item/%E6%B4%BB%E5%8A%A8%E7%AA%97%E5%8F%A3?fromModule=lemma_inlink)保持活动。 |
| SW\_SHOWNA | 8 | 以当前状态显示窗口。活动窗口保持活动。 |
| SW\_RESTORE | 9 | 激活并显示窗口。若窗口是最小化或最大化，则恢复到原来的大小和位置。在还原应用程序的最小化窗口时，应指定该标志。 |

blnWaitOnReturn

如果未指定 blnWaitOnReturn 或其值为 FALSE，则该方法立即返回到脚本继续执行而不等待进程结束。

若 blnWaitOnReturn 设为 TRUE，则 Run 方法返回由应用程序返回的任何错误代码。如果未指定 blnWaitOnReturn 或其值为 FALSE，则 Run 返回错误代码 0（zero）。

示例
~~~java
'这段用记事本打开当前执行的脚本
Dim WshShell, ErrNumber
Set WshShell = CreateObject("WScript.Shell")
WshShell.Run("notepad " & Wscript.ScriptFullName)
WshShell.Run("%windir%\notepad.exe " & Wscript.ScriptFullName)
'这段返回执行的应用程序的错误码（退出码）
ErrNumber = WshShell.Run("notepad " & Wscript.ScriptFullName, 1, True)
~~~

[  
](https://baike.baidu.com/pic/wscript.shell/7752223?fr=lemma)