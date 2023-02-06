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
【MRSServer关键版本】
1.0.14开始支持4网卡，兼容2网卡，指导手册参考：E:\Source\MM\ReleaseNote\MRS-V1.0.14.20220213sp1发布说明.docx
1.0.15SP2（含）开始，系统需要升级到Ubunt18.04



重建矩阵，256开始每次加256，一直加到1024，1024开始每次加512















