前言
为什么要去学习探究微软的PowerShell？？？我们都知道大多高手来自 .bat | .vbs 脚本，要知道cmd只是适用于普通人，不过Shell工具中不是还有Bash吗？那有没有继承两者优势的东西，既功能强大而且逼格比较高级的玩意儿？答案是：有的，就是PowerShell，真香！实不相瞒我之前是学习了一些php语法基础，PowerShell它的语法与其是有很多相似的地方或者说有些地方是一毛一样（夸张啦~[滑稽]）；还有PowerShell它来自C#能调用.Net对象进行操作。

REVISE: msdos VS powershell VS cmd VS windows script host VS powershell core VS console-.net
​somabright.com/2018/11/22/revise-msdos-vs-powershell-vs-cmd-vs-windows-script-host-vs-powershell-core-vs-console-net/

PowerShell内置可扩展的cmdlet命令，cmd常用的命令同样它也能用(cd | dir | copy | move | cls | echo)
不过这里就有点区别，这些只是cmdlet命令的别名；那是怎样知道这些英文简写是某些cmdlet命令的别名呢？

Get-Alias 获取别名

Get-Alias -Name cd       # 查看cd是什么命令类型
gal cd  # 单个别名查询 [gal -> get-alias]，gal是get-alias别名

返回结果 ：
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------  
Alias          cd -> Set-Location

gal cd,dir,copy,move,cls,echo # 多个别名查询，用“,”隔开即可

返回结果 ：
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           cd -> Set-Location
Alias           dir -> Get-ChildItem
Alias           copy -> Copy-Item
Alias           move -> Move-Item
Alias           cls -> Clear-Host
Alias           echo -> Write-Output
Get-Command 获取全部cmdlet、Function函数和Alias别名，范围更大

Get-Command -Name Set-Location # 查看Set-Location是什么命令类型
gcm set-location # 单个cmdlet查询 [gcm -> get-command]，gcm是get-command别名
gcm sl # sl是set-location的别名，所以这里跟gal sl结果是一样的；
gcm set-location,get-command # 多个cmdlet命令查询 

返回结果 ：
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Set-Location
Get-Help 获取cmdlet命令的参数名称

Get-Help -Name Get-Command # 查看Get-Command的参数名称以及参数类型
help gcm # 这里只能单个cmdlet命令查询，可以用help help查看相应参数名称和参数类型
help get-command # (等同于help gcm ) 查找帮助无论用别名还是cmdlet命令的全拼，结果同上
学到这里，或许你会发现gal是get-alias的别名，gcm是get-command的别名；而help是不是get-help的别名呢？？？还有你是如何知道那些cmdlet的别名的呢……


？？？
前面提到gcm查询的范围很大，包含cmdlet命令、Alias别名和Function函数；机智的同学早已发现了不对劲，目前已经出现了cmdlet命令和Alias别名，还有一个Function……好，很好；现在可以回答help是不是get-help的别名，首先我们不难发现gal -> get-alias、gcm -> get-command 都是三个字母而已，前两个取单词首字母，然后就是你猜第三个字母会取啥，对吧。到这里help肯定就不是get-help的别名，那会不会是Function呢
gal help # 会报错！！！

gal : 此命令找不到匹配的别名，因为具有 name“help”的别名不存在。
所在位置 行:1 字符: 1
+ gal help
+ ~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (help:String) [Get-Alias], ItemNotFoundException
    + FullyQualifiedErrorId : ItemNotFoundException,Microsoft.PowerShell.Commands.GetAliasCommand

gcm help # 用get-command查询一下

返回结果 ：
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Function      help
利用Get-Alias查找cmdlet命令的别名

gal ga*,gc* # 已知别名的前两个字母，进行多个模糊查询

# 这里不用加上-Name这个参数名称，是因为gal默认自带首个参数名称Name 

返回结果 ：
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias         gal -> get-alias
Alias         gcm -> get-command

gal -definition "get-process" # 完全未知的cmdlet命令进行查找其别名，可以通过-Definition参数名称

<# 若这里不带上-Definition，就直接变成 gal "get-process";PowerShell会报错，报什么错误!??

gal : 此命令找不到匹配的别名，因为具有 name“get-process”的别名不存在。
所在位置 行:1 字符: 1
+ gal "get-process"
+ ~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (get-process:String) [Get-Alias], ItemNotFoundException
    + FullyQualifiedErrorId : ItemNotFoundException,Microsoft.PowerShell.Commands.GetAliasCommand

有没有发现这名话name“get-process”的别名不存在，验证了gal自带-Name参数名称；所以省略也不会发生错误，
命令也能正常执行返回相应结果
#>
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           gps -> Get-Process
Alias           ps -> Get-Process
Cmdlet命令的命名规则是 动词-名词，Get-Command可以根据动词来查看相应的cmdlet命令；那么是否能获取全部cmdlet命令的动词呢？（答案是：有的）

get-verb # 获取全部cmdlet命令的动词

返回结果 ：
Verb        Group         
----        -----         
Add         Common       # 添加 
Clear       Common       # 清除 
Close       Common       # 关闭
Copy        Common       # 复制 
Enter       Common       # 输入 
Exit        Common       # 退出 
Find        Common       # 查找 
Format      Common       # 格式化 
Get         Common       # 获取 
Hide        Common       # 隐藏 
Join        Common       # 连接 
Lock        Common       # 锁 
Move        Common       # 移动 
New         Common       # 创建 
Open        Common       # 打开 
Optimize    Common       # 优化 
Pop         Common       # 弹出 
Push        Common       # 放入 
Redo        Common       # 准备 
Remove      Common       # 移除 
Rename      Common       # 重命名 
Reset       Common       # 重设 
Resize      Common       # 重置大小 
Search      Common       # 搜索 
Select      Common       # 查询 
Set         Common       # 设置 
Show        Common       # 显示 
Skip        Common       # 忽略 
Split       Common       # 拆分 
Step        Common       # 步数
Switch      Common       # 开关 
Undo        Common       # 撤消 
Unlock      Common       # 解禁，解锁 
Watch       Common       # 观看 
Backup      Data         # 返回 
Checkpoint  Data         # 关口
Compare     Data          
Compress    Data          
Convert     Data         # 转换
ConvertFrom Data         # 转换格式为 
ConvertTo   Data         # 转换为
Dismount    Data          
Edit        Data         # 编辑 
Expand      Data          
Export      Data          
Group       Data         # 组化 
Import      Data         # 导入 
Initialize  Data         # 实例 
Limit       Data         # 限制  
Merge       Data          
Mount       Data          
Out         Data         # 输出 
Publish     Data         # 公布 
Restore     Data          
Save        Data         # 保存 
Sync        Data          
Unpublish   Data          
Update      Data         # 更新  
Approve     Lifecycle     
Assert      Lifecycle    # 断言
Complete    Lifecycle     
Confirm     Lifecycle    # 确认 
Deny        Lifecycle     
Disable     Lifecycle    # 禁用 
Enable      Lifecycle    # 激活 
Install     Lifecycle    # 安装 
Invoke      Lifecycle     
Register    Lifecycle    # 注册 
Request     Lifecycle    # 请求 
Restart     Lifecycle    # 重来 
Resume      Lifecycle     
Start       Lifecycle    # 开始 
Stop        Lifecycle    # 停止 
Submit      Lifecycle    # 提交 
Suspend     Lifecycle     
Uninstall   Lifecycle    # 取消安装  
Unregister  Lifecycle    # 取消注册 
Wait        Lifecycle    # 等待 
Debug       Diagnostic   # 调试 
Measure     Diagnostic    
Ping        Diagnostic   
Repair      Diagnostic   # 修补 
Resolve     Diagnostic    
Test        Diagnostic   # 测试 
Trace       Diagnostic    
Connect     Communications
Disconnect  Communications
Read        Communications   # 读取
Receive     Communications
Send        Communications   # 发送
Write       Communications   # 写入
Block       Security      
Grant       Security      
Protect     Security      
Revoke      Security      
Unblock     Security      
Unprotect   Security      
Use         Other        # 使用
# 剩下的不常用，有需要微软官网自查

<# 这……好像好多，到底多少个呢？肯定是98个，不要问我怎么知道的【捂脸】【捂脸】 #>
get-verb | fl verb|group

返回结果 ：
Count Name                      Group                                                                                                          
----- ----                      -----                                                                                                          
    1 Microsoft.PowerShell.C... {Microsoft.PowerShell.Commands.Internal.Format.FormatStartData}                                                
    1 Microsoft.PowerShell.C... {Microsoft.PowerShell.Commands.Internal.Format.GroupStartData}                                                 
   98 Microsoft.PowerShell.C... {Microsoft.PowerShell.Commands.Internal.Format.FormatEntryData, Microsoft.PowerShell.Commands.Internal.Forma...
    1 Microsoft.PowerShell.C... {Microsoft.PowerShell.Commands.Internal.Format.GroupEndData}                                                   
    1 Microsoft.PowerShell.C... {Microsoft.PowerShell.Commands.Internal.Format.FormatEndData} 

# 第三行已经给出答案了

<# 第二种方法
设置格式为group-object 组化
sort 排序
#>

get-verb | group-object sort

返回结果 ：
Count Name                      Group
----- ----                      -----
   98                           {@{Verb=Add; Group=Common}, @{Verb=Clear; Group=Common}, @{Verb=Close; Group=Common}, @{Verb=Copy; ...

# 同样也可以得出98个
Get-Command 可以根据(动词/名词)来查询cmdlet命令，参数-Verb 动词，-Noun 名词

gcm -verb get # 查询get动词开头的cmdlet

返回结果 ：
CommandType     Name                                               Version    Source                                                  
-----------     ----                                               -------    ------                                                  
Alias           Get-DiskSNV                                        2.0.0.0    Storage                                                 
Alias           Get-PhysicalDiskSNV                                2.0.0.0    Storage                                                 
Alias           Get-ProvisionedAppxPackage                         3.0        Dism                                                    
Alias           Get-StorageEnclosureSNV                            2.0.0.0    Storage                                                 
Function        Get-AppBackgroundTask                              1.0.0.0    AppBackgroundTask                                       
Function        Get-AppvVirtualProcess                             1.0.0.0    AppvClient                                              
Function        Get-AppxLastError                                  2.0.0.0    Appx                                                    
Function        Get-AppxLog                                        2.0.0.0    Appx                                                    
Function        Get-AssignedAccess                                 1.0.0.0    AssignedAccess                                          
Function        Get-AutologgerConfig                               1.0.0.0    EventTracingManagement                                  
Function        Get-BCClientConfiguration                          1.0.0.0    BranchCache                                             
Function        Get-BCContentServerConfiguration                   1.0.0.0    BranchCache                                             
Function        Get-BCDataCache                                    1.0.0.0    BranchCache                                             
Function        Get-BCDataCacheExtension                           1.0.0.0    BranchCache                                             
Function        Get-BCHashCache                                    1.0.0.0    BranchCache                                             
Function        Get-BCHostedCacheServerConfiguration               1.0.0.0    BranchCache                                             
Function        Get-BCNetworkConfiguration                         1.0.0.0    BranchCache                                             
Function        Get-BCStatus                                       1.0.0.0    BranchCache      
……（这里省略很多）

gcm -noun computer # 根据名词computer查询cmdlet

返回结果 ：
CommandType     Name                                               Version    Source                                                  
-----------     ----                                               -------    ------                                                  
Cmdlet          Add-Computer                                       3.1.0.0    Microsoft.PowerShell.Management                         
Cmdlet          Checkpoint-Computer                                3.1.0.0    Microsoft.PowerShell.Management                         
Cmdlet          Remove-Computer                                    3.1.0.0    Microsoft.PowerShell.Management                         
Cmdlet          Rename-Computer                                    3.1.0.0    Microsoft.PowerShell.Management                         
Cmdlet          Restart-Computer                                   3.1.0.0    Microsoft.PowerShell.Management                         
Cmdlet          Restore-Computer                                   3.1.0.0    Microsoft.PowerShell.Management                         
Cmdlet          Stop-Computer                                      3.1.0.0    Microsoft.PowerShell.Management                         
 
好像遗漏了什么，哦，应用程序


嘿嘿嘿
若有同学安装过vim或neovim，并设置环境变量；就可以使用cat命令查看文件内容。没有安装过也没有关系，可以查询cmd或powershell可执行文件存放的位置

gcm -name cat -commandtype application # 查询cat可执行文件的绝对路径
gcm -name cmd -commandtype application # 查询cmd可执行文件的存放位置
gcm -name powershell -commandtype application # 查询powershell可执行文件的存放位置
编辑于 2021-12-23 13:56