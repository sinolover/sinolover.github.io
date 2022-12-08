# 消息信道

|        模块         |         消息         | 消息号  |                 子过程                  |            说明            |
| ------------------- | -------------------- | ------ | -------------------------------------- | -------------------------- |
| CommunicationModule | SendRSReconStatus    | 0x8023 | =>nvRBServer::ResponseGetReconState    |                            |
| CommunicationModule | SendReconResult      | 0x8030 |                                        |                            |
| CommunicationModule | SendRSDevStatus      | 0x8005 |                                        | 设备状态通知（0x8005）        |
| CommunicationModule | SendQuickReconStatus | 0x8029 |                                        | 返回实时重建状态通知(0x8029) |
| CommunicationModule | SendDelStatus        | 0x8013 | =>nvRBServer::PostResponse             |                            |
| CommunicationModule | SendDevStatus        | 0x8022 | =>nvTTServer::ResponseDeviceState      |                            |
| CommunicationModule | SendRSError          | 0x8060 | =>nvRBServer::ReportErrorMessage       |                            |
| CommunicationModule | SendDevError         | 0x8060 | =>nvRBServer::ReportErrorMessage       |                            |
| CommunicationModule | SendAllError         | 0x8060 | =>nvRBServer::ReportErrorMessage       |                            |
| CommunicationModule | SendLogToLog         | 0x8040 | =>nvRBServer::SendLog                  |                            |
| CommunicationModule | SendDebugLog         | 0x8040 | =>nvRBServer::SendLog                  |                            |
| CommunicationModule | SendInfoLog          | 0x8040 | =>nvRBServer::SendLog                  |                            |
| CommunicationModule | SendWarnLog          | 0x8040 | =>nvRBServer::SendLog                  |                            |
| CommunicationModule | SendErrorLog         | 0x8040 | =>nvRBServer::SendLog                  |                            |
| CommunicationModule | SendFatalLog         | 0x8040 | =>nvRBServer::SendLog                  |                            |
| nvBaseServer        | SendFSDevStatus      | 0x8005 | =>nvTTServer::ResponseDeviceState      | 设备状态通知0x8005          |
| nvBaseServer        | SendWorkFlowStatus   | 0x8023 | => m_pCommModule->SendDevState         |                            |
| nvBaseServer        | SendReconStatus      | 0x8023 | => m_pCommModule->SendQuickReconStatus | 重建进度                    |
| nvBaseServer        | SendReconResult      | 0x8023 | => m_pCommModule->SendReconState       |                            |
| nvBaseServer        | SendScanStatus       | 0x8023 | => m_pCommModule->NotifyScanState      |                            |
| nvBaseServer        | SendRSStatus         | 0x80XX |                                        | 空代码                     |
| nvServer            | SendScanStatus       | 0x8023 | => m_pCommModule->NotifyScanState      |                            |