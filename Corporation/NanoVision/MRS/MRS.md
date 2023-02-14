# 代码结构分析


**mermaid**　扩展词汇　
- 英 ['mɜːmeɪd] 　 　 美 ['mɜːrmeɪd] 　 　
- n. 美人鱼
说明：
1. 圆圈：变量
2. 长方形：类、实例
3. 六边形：硬件（网卡）
4. 梯形：服务器、终端（CTBox、ImgTrans）
5. 圆形：交换机
6. 

硬件相关（空白部分可以插入后面的变量相关，整个图形不变性）

~~~mermaid
graph TD
  nvServer[g_pServer:nvServer_nvBaseServer]--初始化并传递gServer-->commModule[m_pCommModule:commModule]
  commModule--初始化<br/>注册回调-->nvTTServer[m_SocketDevServer:nvTTServer]--HB:10017<br>CMD:10027-->RJ45_MCS{{RJ45 1G 千兆<br>192.168.1.1}}-->MCS[/MCS服务器<br>192.168.1.2\]
  commModule--初始化<br/>注册回调-->nvRBServer[m_SocketReconServer:nvRBServer]--SCH:10087<br>DCH:10088-->RJ45_MCS

  nvServer==进程外启动==>nvUpdate[MRSUpdate:nvUpdate]
  nvUpdate--初始化<br/>注册回调-->nvUpdateCmd[nvUpdateCmd]--TCP_10028-->RJ45_MCS
  nvUpdate-->nvUpdateCmd-->nvTelnet[nvTelnet]--TCP_23-->RJ45_CTBox
  nvUpdateCmd-->nvUdpSocket--UDP_8998-->RJ45_CTBox


  nvServer-.->nvControl[m_pControl:Control]

  nvServer--初始化并传递gServer-->nvScanModule[m_pScanModule:ScanModule]
  nvScanModule-->nvControl-->m_pCtBox[nvCtBox回调转化为onBurstMessage]--注册回调MessageHandler-->_gvcp[nvGvcp]--UDP_10086<br>UDP_3956-->RJ45_CTBox{{RJ45 1G 千兆<br/>78.86.65.11}}-->switch((机架上的8口交换机))
  switch-->CTBox[/机架上的CTBox<br>78.86.65.16\]
  switch-->IFBox[/机架上的IFBox<br>78.86.65.17\]
  switch-->mtCtrl[/机架上的mtCtrl<br>78.86.65.21\]
  switch-->pdu[/电源柜上的PDU板<br>78.86.65.19\]
  switch-->tubeIntf[/逆变器上的tubeIntf<br>78.86.65.41-46\]
  nvScanModule-->nvImageAcq

  nvServer-.->nvImageAcq[m_pImageAcq:ImageAcq]-->pcap[nvPcap]--UDP_13394-13409:30806-->Fibra_Detector{{Fibra 10G 万兆<br>78.86.66-81.200}}-->Detector[/机架上的ImgTrans<br>78.86.66.64-71<br>78.86.67.72-79\]

 
~~~


说明：
1. ScanModule
   a.通过调用m_pControl,与 nvControl(CTBox)模块进行交互，将扫描下参到硬件，曝光等大项功能 ，需要执行的几个硬件控制操作进行封装，供nvServer调用。同时，CTBox将收到的BurstMessage，直接转给nvServer进行处理
   b.通过调用m_pImageAcq 获取图像数据
   m_pControl(nvControl) 通过调用nvCTBox(nvCTBox 调用 nvGvcp 来主动连接 nvCtBoxXml：78.86.65.16:3956  MCS:10086，通过寄存器地址，直接获取数据)  -> 电口 -> CTBox -> Gantry
CTBox注册MessageHandler->OnBurstMessage->emit BurstMessage->control connect(CTBox,BusrstMessage,this,BurstMessage)->nvServer connect(control,BusrstMessage,this,OnBurstMessage)

nvImgAcq:
1. m_pContext：全局上下文，故流程图中忽略
2. utility：全局工具，故流程图中忽略
2. m_pControl：只使用到了workmode函数，且只是获取状态，故流程图中忽略
3. nvGige：初始化pcap

nvControl：
1. nvCtBox


GigE Vision标准由4个主要部分组成[1]：
　(1)定义了让应用程序发现和枚举设备的机制，定义了设备如何获得一个有效的IP地址；
　(2)定义了GigE Vision控制协议(GVCP)，允许对被发现的设备进行配置，保证传输的可靠性；
　(3)定义了GigE Vision流协议(GVSP)，允许应用程序接收发自设备的信息；
　(4)定义了bootstrap寄存器，描述了设备自身，如当前IP地址、序列号、制造商信息等。


变量相关
~~~mermaid
graph TD
  nvServer[g_pServer:nvServer_nvBaseServer]

  nvServer--nvBaseServer中初始化-->nvContext>m_pContext:Context]
  nvServer--nvServer中初始化-->nvImgCorrMgr[m_pImgCorrMgr:ImgCorrMgr]-->nvImgCorr[m_pImgCorr:ImgCorr]
~~~



3.CommonModule
功能：注册回调函数，将CmdProcess模块反馈上来的信息，转发给nvBaseServer层处理；将Server反馈结果和状态等，转发给CmdProcess模块（进而发送给MCS）
P6
    SendInfoLog->SendLogToLogCenter(“Info”…)  ->
                                                                                            LOG4CXX_INFO(serverLog,) && RBSendLog
    SendErrLog -> SendLogToLogCenter(“Error“…)->

                               MRSUpdateCmd()(对其他设备进行升级)
4.MRSUpdate->  调用nvTelnet 对MRSUpdatecmd 准备好的设备进行远程升级
                               UpdateDetectorImpl(string,string)->nvUpdate :8998UDP -> Detector(仅支持UDP，对Detector进行升级)


5.nvBaseServer
功能：MRSServer主程序的基类，有两大作用：
  a.作为模拟采集重建服务进行使用（可能现在不能正常用了）
  b.作为真机采集重建服务的父类，实现非真机扫描采集的其他功能，如修改病人信息，离线重建等。

6.nvServer
功能：MRSServer真机采集重建服务类，用于实现真机环境下采集图像，binning4x4校正及RTD重建，以及处理CTBox反馈的各种状态信息。


7.nvUpdate -> m_pControl(nvControl)


CONTROL(TCP) 10027 m_nServerDevCmdPort
CMD(TCP)         10087 m_nServerReconCmdPort
DATA(TCP)        10088 m_nServerReconCmdPort

Console:                控制台，              简称C-S
Rebuilder:             重建电脑，          简称R-S
Firmware:             TT设备控制板，简称F-S
Signal Channel：信令通路,              简称 SCH
Data Channel：  数据通路，           简称DCH

*****
MCS和MRS的通信协议定义CMD Channel以及DATA Channel，不定义Control Channel。
*****
控制台与设备控制（TTServer  Console TCP ）的通信协议

Res\MachineChip2Src24\nvServerConf.xml
10027 MCS->MRS 设备控制端的通讯
10087 MCS->Box CTBox的通讯端口（UDP）CTBox：3956 配置文件（Res/MachineChip2Src24/nvCtBox.xml）
10088 MCS->MRS 图像及raw数据的通讯端口


10028 MCS->MRSUpdate 升级程序的通讯端口
10086 MCS->MRS 数据重建指令端的通讯端口（未用）

       MRS              探测器
78.86.66.200: 13394 -> 78.86.66.64: 30806
...   ...
78.86.66.200: 13401 -> 78.86.66.71: 30806
78.86.67.201: 13402 -> 78.86.67.72: 30806
...   ...
78.86.67.201: 13409 -> 78.86.67.79: 30806


Res\MachineChip2Src24\nvDicomParam.xml
        <DicomIP>192.168.20.100</DicomIP>
        <DicomPort>3002</DicomPort>

    if (m_pContext->m_nMachineId == MACHINE_CHIP1_SRC6)
    {
        bool pduRet = m_pduGvcp.Open(param.LocalIP, 26, "78.86.65.26", param.RemotePort);

libs/nvComm/nvContext 存放/Res/MachineChip2Src24下定义的全局变量：
    int m_nChipImgWidth;    //单芯片图像宽度
    int m_nChipImgHeight;   //单芯片图像高度
    int m_nModuleImgWidth;  //单模组图像宽度
    int m_nModuleImgHeight; //单模组图像高度
    int m_nRingImgWidth;    //整环图像宽度
    int m_nRingImgHeight;   //整环图像高度
    int m_nCutImgWidth;     //裁剪图像宽度
    int m_nCutImgHeight;    //裁剪图像高度


  //图像校正接口
  m_pImgCorrMgr = nvImgCorrMgr::GetInstance();
  //图像采集类
  m_pImageAcq = nvImgAcq::GetInstance();
  //CTBOX控制类
  m_pControl = nvControl::GetInstance();
nvcommEx.h   错误代码定义
nvcommEx.cpp 错误消息定义

日志：
dtkLog：nvTrace()  nvDebug()  nvInfo()  nvWarn()  nvError()   nvFatal()
nvInfo()   ->Log/YYYY-MM-DD-nvServer.log        dtkLogger::instance().attachConsole(); nvBaseServer
                 ->Log/YYYY-MM-DD-MRSUpdate.log  dtkLogger::instance().attachConsole(); nvUpdate
   
nvError() ->scanModule commModule nvParseConf nvImgAcq

qDebug   ->nvImgAcq

LoggerPtr cmdLog        = Logger::getLogger("CmdProcess"); Log/CmdProcess -YYYY-MM-DD
LoggerPtr serverLog      = Logger::getLogger("Server");            Log/nvServer-yyyy-MM-DD 
LoggerPtr ctboxLog       = Logger::getLogger("CTBox");             Log/CTBox -yyyy-MM-DD
LoggerPtr reconLog      = Logger::getLogger("Recon");             Log/Recon-YYYY-MM-DD
LoggerPtr errorwarLog  = Logger::getLogger("ErrorWarn");     Log/ErrorWarn-yyyy-MM-DD

热熔分级：0-10：警告 10-80：正常 90-100：警告
    for (size_t i = 0; i < m_nSourceNum; i++)
        tubeInfo.tubeHeat = m_pContext->m_vPostInfo[i].pPercentOfHeat; 
      }
    }


输入完病人姓名年龄等病人信息，点击Exam时，MCS生成StudyID
选择完扫描协议，并点击 Confirm 时，MCS生成ScanID
点击Go，MCS生成SeriesID


# 代码分析
nvServer
1.Server启动的所有线程  
~~~cpp
  //定时查询CTBox的心跳及各部件的状态
  StartThread(THREAD_CTBoxTimeout);
  m_pCommModule->StartRecvDevMsgThread(m_strServerIP, m_nServerDevCmdPort, 10, m_sConfigPath);
  m_pCommModule->StartRecvReconMsgThread(m_strServerIP, m_nServerReconCmdPort, m_nServerDataPort, 10, m_sConfigPath);
~~~
2.OnScanPrepAction 中设置所有扫描参数
~~~cpp
SetupScanParam-MotionSetting-StartScan-WSReady
  //高压注射器
  m_pScanModule->SetInjectorEnable(false);
  //帧时间
  m_pScanModule->SetFrameTime();
  auto darkImgVec = m_pScanModule->AcqDarkImage(m_nDarkFrameNum);
  m_pImgCorrMgr->CreateOffsetFromRawImg(darkImgVec); //


  m_pScanModule->SetAECParams(m_bAECEnable);
  m_pScanModule->SetupScanParam(m_nExpEnable); //KV 扫描范围都需要重设

  m_pContext->UpdateImageSize();
  m_pImgCorrMgr->UpdateCutParam();


  m_pImgCorrMgr->InitCorr()

  m_pScanModule->SetLightStripMode(LS_PREP, m_nLightStripSpeed);


    m_pScanModule->SetInjectorEnable(true);

    //两点轮流曝光
    if (m_nIntervalTubeNumber == 6)
    {
        nNextTubeID = (m_nSelectTubeID + 5) % 24 + 1;
    }
    else
    {
        nNextTubeID = (m_nSelectTubeID + 7) % 24 + 1;
    }
    m_pControl->SetTwoTubeIDA(m_nSelectTubeID, nNextTubeID);

    //两点同时曝光设置
    int nNextTubeID = (m_nSelectTubeID + 7) % 24 + 1;
    m_pControl->SetTwoTubeIDB(m_nSelectTubeID, nNextTubeID);
~~~
3.ImageCallBack,
~~~cpp
  所有收到的Image 放到 m_imageVec 中(该行代码已注释，m_imageVec 定义不存在)
  之后，进行切图(cut选项设置在onScanPrepAction中：m_pImgCorrMgr->UpdateCutParam()), 并且:
    a.定位像、小剂量放入 `m_cutImgVec`
    b.螺旋扫、轴扫放入 `m_vvecCutImgVec`
~~~
4.DoQuickRecon 0x20 包括矫正和重建
~~~cpp
  a.StartRTDProjImagesThread[etCorrOptions] -> CreateRTDProjImage[ImageBin CorrImageGpuBinning];
    先 binning 然后 correct
    /这里的矫正是第二次初始化矫正，第一次在 nvBaseServer::StartService() 里执行
    m_pImgCorrMgr->InitCorr();
    //设置矫正参数
    m_pImgCorrMgr->SetCorrOptions(corrOption);
    m_pImgCorrMgr->InitCorrBinning(); //参数设置 m_pImgCorrMgr->UpdateCutParam();  在 nvBaseServer::StartService() 里执行    
    //分别开始 矫正线程
                            ImageCallback       CreateProject(投影图)                                                      Recon
    CreateScoutProjImage  -> m_cutImgVec     -> 校正后(m_pImgCorrMgr->CorrImageGpu(cutImg))数据存入 m_vecProjImg         -> m_vecImgPiece
    CreateSubtractImage   -> m_cutImgVec     -> 校正后(m_pImgCorrMgr->CorrImageGpu(cutImg))数据存入 m_vecProjImg
    CreateRTDProjImage    -> m_vvecCutImgVec -> 校正后(m_pImgCorrMgr->CorrImageGpu(cutImg))数据存入 m_vvecRTDProjImg
    
    Utility::SaveRaw(*projImg, strFilePath.toStdString().c_str());

  b.StartQuickReconsThread(); 
    -> QuickReconImages  
       ->AXIAL_SCAN:AxialBinningReconProcess();         重建数据存放在: m_recImgVec.clear();
         SCOUT_SCAN:nvBaseServer::ScoutReconProcess();  重建数据存放在: m_vecImgPiece.clear(); OnSendScoutPiece 发送dicom数据 OnCreateScoutView 生成最终定位像的View图
        SPIRAL_SCAN:SpiralBinningReconProcess();        重建数据存放在: m_recImgVec.clear();
        
        AXIAL_SCAN、SPIRAL_SCAN 均使用以下重建过程
        m_pReconSpiral = new ReconstructSpiral(this->m_pContext->m_HctInfo);
        m_pReconSpiral->CFDK_Init(projImg);
        m_pReconSpiral->CFDK_Recon(projImg);
        m_pReconSpiral->CFDK_Exit(m_recImgVec)  重建完成后，重建好的图放在 m_recImgVec 中(初始化的时候已经清空过)

    ->nvBaseServer::QuickReconImages
      nvBaseServer::AxialReconProcess
      nvBaseServer::ScoutReconProcess
      nvBaseServer::SpiralReconProcess
~~~        
5.DoSeqRecon 0x21 生成的数据存放在 vecSliceImg 中 
~~~cpp
    -> StartThread(THREAD_SEQRECON == eThreadType)
       -> thread(nvBaseServer::onOfflineReconstruct) 加载重建参数  加载磁盘 Raw 或者 Proj 数据
          -> nvBaseServer::StartOfflineReconsThread   仅仅开了一个新的线程 OfflineReconImages
             -> thread(vBaseServer::OfflineReconImages)
                -> 首先 RemoveBoneArtifact 或者 RemoveMetalArtifact(二者二选一，RemoveBoneArtifact 优先)
                -> AXIAL_SCAN:  AxialReconProcessNew2                           StartCFDK
                -> SPIRIAL_SCAN: FBP:SpiralReconProcessNew IVR:SpiralReconIVR   StartHFDK
SendFSDevStatus
SendRSDevStatsu


扫描_API-导航灯-SetupScanParam-MotionSetting-StartScan-WSReady-三期
~~~



# 消息信道

|                一级调用                 |     模块      |                消息                 | 消息号  |                   子过程                    |          说明           |
| -------------------------------------- | ------------ | ---------------------------------- | ------ | ------------------------------------------ | ----------------------- |
| BS::GetReconStatus //0023 查询重建状态  | CommModule   | SendRSReconStatus                  | 0x8023 | =>nvRBServer::ResponseGetReconState        |                         |
|                                        | CommModule   | SendRSDevStatus                    | 0x8005 |                                            | 设备状态通知（0x8005）   |
| BS::SendReconStatus(SendReconProgress) | CommModule   | SendQuickReconStatus               | 0x8029 | =>nvRBServer::ResponseReconStateLiveNotify | 返回重建进度通知(0x8029) |
| BS::SendReconResult                    | CommModule   | SendReconResult                    | 0x8030 |                                            | 返回重建状态通知(0x8030) |
|                                        | CommModule   | SendDelStatus                      | 0x8022 | =>nvRBServer::ResponseDelStatus            |                         |
|                                        | CommModule   | SendDevState                       | 0x8023 | =>nvTTServer::ResponseDeviceState          |                         |
|                                        | CommModule   | SendRSError                        | 0x8060 | =>nvRBServer::ReportErrorMessage           |                         |
|                                        | CommModule   | SendDevError                       | 0x8060 | =>nvRBServer::ReportErrorMessage           |                         |
|                                        | CommModule   | SendAllError                       | 0x8060 | =>nvRBServer::ReportErrorMessage           |                         |
|                                        | CommModule   | SendLogToLog                       | 0x8040 | =>nvRBServer::SendLog                      |                         |
|                                        | CommModule   | SendDebugLog                       | 0x8040 | =>nvRBServer::SendLog                      |                         |
|                                        | CommModule   | SendInfoLog                        | 0x8040 | =>nvRBServer::SendLog                      |                         |
|                                        | CommModule   | SendWarnLog                        | 0x8040 | =>nvRBServer::SendLog                      |                         |
|                                        | CommModule   | SendErrorLog                       | 0x8040 | =>nvRBServer::SendLog                      |                         |
|                                        | CommModule   | SendFatalLog                       | 0x8040 | =>nvRBServer::SendLog                      |                         |
|                                        | CommModule   | NotifyScanState                    | 0x801f | =>nvTTServer::NotifyScanState              | 扫描状态通知0x801f       |
|                                        | nvBaseServer | SendFSDevStatus                    | 0x8005 | =>nvTTServer::ResponseDeviceState          | 设备状态通知0x8005       |
|                                        | nvBaseServer | SendWorkFlowStatus SendFSDevStatus | 0x8023 | =>CommModule->SendDevState                 | TT:ResponseDeviceState  |
|                                        | nvBaseServer | SendReconStatus                    | 0x8029 | =>CommModule->SendQuickReconStatus         | 重建进度                 |
|                                        | nvBaseServer | SendReconResult                    | 0x8030 | =>CommModule->SendReconState               |                         |
|                                        | nvBaseServer | SendScanStatus                     | 0x801f | =>CommModule->NotifyScanState              |                         |
|                                        | nvBaseServer | SendRSStatus                       | 0x80XX |                                            | 空代码                  |
|                                        | nvServer     | SendScanStatus                     | 0x801f | =>CommModule->NotifyScanState              |                         |

8013
# MRSServer关键版本
1.0.14开始支持4网卡，兼容2网卡，指导手册参考：E:\Source\MM\ReleaseNote\MRS-V1.0.14.20220213sp1发布说明.docx
1.0.15SP2（含）开始，系统需要升级到Ubunt18.04



重建矩阵，256开始每次加256，一直加到1024，1024开始每次加512















