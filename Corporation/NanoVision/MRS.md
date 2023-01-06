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

重建矩阵，256开始每次加256
         1024开始每次加512















