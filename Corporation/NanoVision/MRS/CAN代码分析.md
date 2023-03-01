一、代码整体结构
【CAN相关】
1.CanCommunication 	  无关代码
2.CanCommunicationForPro  Can通讯部分代码 cancommunication.dll
3.ClassForView 		  无关代码
4.CommandTest1 		  无关代码
5.ConsoleApp2  		  无关代码
6.GIBoardTest  		  无关代码
7.MCSCommunication_CAN    无关代码
8.UITest       		  无关代码
【TCP相关】
1.MCSCOmmunication_TCP    MCS通讯部分代码 mcscommunication.dll
2.MRSCommunication        MRS通讯部份代码 mrscommunication.dll
3.TCPTEST      		  MCSCOmmunication_TCP主程序中引用，用来做测试
【测试】
1.Test
【逻辑相关】
1.ConsoleApp1  		  无关代码
2.CTCommunication 	  无关代码 测试代码
3.EventCenter  		  无关代码
4.MCS_MRS_STR  		  待查    mcs_mrs_str.dll
5.TestLogic    		  无关代码
6.Setup_CTCommunication1  安装程序 
  使用了微软的插件：Microsoft Visual Studio Installer，因此打开解决方案前，需要先安装插件，否则该项目打不开
[SQLHelper]		  数据库Helper



二、代码分析
【CanCommunicationForPro】
					 RecvDataThread.cs                          BoxProtocol.cs
1.zlgcan.dll->zlgcan.cs->CanProtocol.cs->                         [CanFrame]     -> GIBoardProtocol.cs
					 CanDeviceWorkThread.cs                     ControlPanleProtocol.cs


~~~mermaid
graph LR
zlgcan[zlgcan.dll]-->zlg[zlgcan.cs]-->RecvDataThread[RecvDataThread.cs]
zlg-->CanProtocol[CanProtocol.cs]
zlg-->CanDeviceWorkThread[CanDeviceWorkThread.cs]
CanProtocol-->BoxProtocol[BoxProtocol.cs]
CanProtocol--CANFrame-->GIBoardProtocal[GIBoardProtocal.cs]
CanProtocol-->ControlPanleProtocol[ControlPanleProtocol.cs]
~~~


【MCSCOmmunication_TCP】


【MRSCommunication】
1.定位像数据拼接详解
2.



三、软件探测
【运行VT0程序，得出如下结果】：
无CanCommunicationForpro.DLL不出界面，报错：
System.IO.FileNotFoundException: Could not load file or assembly 'CanCommunicationForPro.dll

无MRSCommunication.dll出界面，停在【开始自检】界面，报错：
全局异常System.IO.FileNotFoundException: Could not load file or assembly 'MRSCommunication.dll

无MCSCommunication.dll出界面，停在【开始自检】界面，报错：
全局异常System.IO.FileNotFoundException: Could not load file or assembly 'MCSCommunication.dll

无MCS_MRS_STR.dll，目前未发现有什么影响

【编译VT0  17版本，得出一下结论】
无MCS_MRS_STR.dll引用，编译成功
【C_SRC代码】
1.CANCommunicationforpro 仅 额外引用了 Log4net
2.MRSCommunication 引用了：log4net MySql.Data sqlSugar SQLHelper TCPTEST(经测试，本引用未使用，不影响编译)








