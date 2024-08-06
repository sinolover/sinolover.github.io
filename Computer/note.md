# note
#include "../SDKs/nvImgAcq/nvcontrol.h"
#include "nvcommEx.h"
// nvControl::nvControl()
// {

// }
// nvControl::~nvControl()
// {




nvControl *nvControl::s_instance = NULL;

nvControl *nvControl::GetInstance()
{
}

nvControl::nvControl()
{





}

nvControl::~nvControl()
{
}

bool nvControl::Connect()
{
}

void nvControl::Disconnect()
{
}

bool nvControl::IsConnect()
{
}

void nvControl::InitParam()
{
}

bool nvControl::MotionEnbale(bool enbale)
{

}

bool nvControl::MotionReady()
{

}

bool nvControl::MotionStart()
{


}

bool nvControl::MotionStop()
{
}

bool nvControl::MotionReturn()
{

}

bool nvControl::SyncAecParam()
{


}

bool nvControl::SetTubeID(int tubeId)
{
}

bool nvControl::SetTwoTubeIDA(int tubeID1, int tubeID2)
{
}
bool nvControl::SetTwoTubeIDB(int tubeID1, int tubeID2)
{
}

bool nvControl::StartRobotMove()
{
}

bool nvControl::StopRobotMove()
{
}

bool nvControl::SetRobotMotionMode(int mode)
{
}

bool nvControl::SetRobotDirection(int value)
{
}

bool nvControl::SetRobotMaxVLCT(uint value)
{
}

bool nvControl::SetRobotAimPST(int value)
{
}

bool nvControl::SetSoftLMTEN(bool enabled)
{
}

bool nvControl::StartBedMove()
{
}

bool nvControl::StopBedMove()
{
}

bool nvControl::SetBedMotionMD(int value)
{
}

bool nvControl::SetBedAimPST(int value)
{
}
bool nvControl::SetBedAimVLCT(int value)
{
}
bool nvControl::SetBedHrztInitPos(int value)
{
}

bool nvControl::SetBedSpDist(int value)
{
}
bool nvControl::SetBedDirction(int value)
{
}

bool nvControl::ClearBedVerticalEncoder()
{
}

bool nvControl::SetBedVerticalAimPos(int value)
{
}

bool nvControl::StartBedVerticalMove()
{
}

bool nvControl::StopBedVerticalMove()
{
}

bool nvControl::ClearBedHorizontalEncoder()
{
}

bool nvControl::SetBedHorizontalParam(float aimPos, float speed)
{


}

bool nvControl::StartBedHorizontalMove()
{


}

bool nvControl::StopBedHorizontalMove()
{




}

bool nvControl::SetBedMoving(BED_MOVE_DIR value)
{


}

bool nvControl::GetBedHorizontalPosition(float &fValue)
{





}

bool nvControl::GetBedVerticalPosition(int &value)
{



}

bool nvControl::GetBedHorizontalSpeed(int &value)
{



}

bool nvControl::GetBedSoftVerMain(int &value)
{



}

bool nvControl::GetBedSoftVerSlave(int &value)
{



}

// LightStrip
bool nvControl::GetLightStripStatus(int &value)
{



}

bool nvControl::GetLightStripVersion(int &value)
{



}

bool nvControl::SetLightStripMode(int value)
{


}

bool nvControl::SetLightStripSpeed(int value)
{


}

bool nvControl::GetBreathLightStatus(int &value)
{



}

bool nvControl::GetBreathLightVersion(int &value)
{



}

bool nvControl::SetBreathLightHold(int value)
{


}

bool nvControl::SetBreathLightExhale(int value)
{


}

bool nvControl::SetWorkStation(bool bReady)
{


}

bool nvControl::GetWorkStation(bool &bReady)
{


}

bool nvControl::StartScan()
{





}

bool nvControl::StopScan()
{





}

bool nvControl::GetWorkMode(int &value)
{



}

bool nvControl::StartScanEx()
{










}

bool nvControl::StopScanEx()
{








}

bool nvControl::IsScanning()
{


}

bool nvControl::SetOverSample()
{














}

bool nvControl::SetExposureEn(int value)
{


}

bool nvControl::SetKV(int kV)
{


}

bool nvControl::SetKV0(int idxKV)
{


}

bool nvControl::SetKV1(int idxKV)
{


}

bool nvControl::GetKV(int sourceGroup, int sourceId, int &value)
{



}

bool nvControl::SetMA(int uA)
{


}

bool nvControl::SetMA0(int uA)
{


}

bool nvControl::SetMA1(int uA)
{


}

bool nvControl::GetMA(int sourceGroup, int sourceId, int &value)
{



}

// Detector
bool nvControl::GetDetectorsVersion(std::vector<DetectorVersion> &vcInfo)
{















}

//tube inverter
bool nvControl::GetTubeInvertersVersion(std::vector<TubeInverterVersion> &vcInfo)
{










}

//allVersion
void nvControl::GetHardwareVersion(HardwareVersion &stVersionInfo)
{
















 

}
bool nvControl::SetKVMACoef()
{




































}

bool nvControl::SetGain(int nGain)
{


}

bool nvControl::SetExpTime(int us)
{


}

bool nvControl::GetExpTime(int sourceGroup, int sourceId, int &value)
{



}

bool nvControl::SetFrameTime(int us)
{


}

bool nvControl::SetFrameNum(int value)
{


}

bool nvControl::SetTriggerMode(TRIGGER_MODE value)
{


}

bool nvControl::SetFocalSpot(FOCAL_SPOT value)
{


}

bool nvControl::GetFocalSpot(int sourceGroup, int sourceId, FOCAL_SPOT &value)
{



}

bool nvControl::SetShutterMode(SHUTTER_MODE value)
{


}

bool nvControl::SetTubeMode(M15_WORKMODE value)
{


}

bool nvControl::SetBinning(int value)
{


}

bool nvControl::SetImageFlip()
{


}

bool nvControl::SaveFlashParam()
{


}

bool nvControl::SetHeartBeat()
{


}

bool nvControl::SetDebugPrint(int value)
{


}

bool nvControl::SetExposureMode(int value)
{


}

bool nvControl::SetPosTrigCtrl(int value)
{


}

bool nvControl::SetPosSet(int value)
{


}

bool nvControl::SetAngleTrigEn(bool enable)
{












}

bool nvControl::SetTableSyncEn(bool enable)
{












}

bool nvControl::SetAngleDirForward(MOTION_DIRECTION value)
{












}

bool nvControl::SetTableDirForward(MOTION_DIRECTION value)
{












}

bool nvControl::SetAngleDirForward(bool enable)
{












}

bool nvControl::SetTableDirForward(bool enable)
{












}

bool nvControl::SetPosTblStart(int value)
{


}

bool nvControl::SetPosTblStop(int value)
{


}

bool nvControl::SetPosExpDist(int value)
{


}

bool nvControl::SetPosFrameDist(float fValue)
{


}

//enable为 0:Auto模式，平时使用的模式
//enable为 1:Command Mode，在此模式下设置AEC参数，波太位置，线束器位置时使用，此模式下没有剂量反馈
bool nvControl::SetAecMode(bool enable)
{
















}

// 等ctBox修改后一同发布
// bool nvControl::SetAecEnable(bool enabled)
// {
//    NVLOG_QUEUE("nvControl.cpp", 0, NVLogFormat("SetAecEnable(): enable=%d.", enabled), 0);
//     int value = 0;
//     m_pCtBox->GetAecSet(value);
//     if (enabled)
//     {
//         value |= (0x01 << 29);
//     }
//     else
//     {
//         value &= (~0x01 << 29);
//     }
//     Utility::DelayMS(50);
//     NVLOG_QUEUE("nvControl.cpp", 0, NVLogFormat("SetAecEnable(): value=%d.", value), 0);
//     return true;//m_pCtBoxx->SetAecSet(value);
// }

bool nvControl::SetAecEnable(bool enable)
{
















}

/*
bool nvControl::SetAecPara(int value)
{

}
*/

bool nvControl::SetEcgSet(int value)
{


}

bool nvControl::SetEcgStartPos(int value)
{


}

bool nvControl::SetEcgStopPos(int value)
{


}

bool nvControl::SetEcgThreshold(int value)
{


}

bool nvControl::GetEcgPerdh(int &value)
{



}

bool nvControl::GetEcgPerdl(int &value)
{



}

bool nvControl::SetAecSrcTube(int value)
{


}

bool nvControl::SetAecFocusTube(int value)
{


}

bool nvControl::SetAecThresh(int value)
{


}

bool nvControl::SetAecFlagMsg(int value)
{


}

bool nvControl::SetAecExec(int value)
{


}

bool nvControl::GetGantryPos(int &value)
{



}

bool nvControl::GetTablePos(int &value)
{



}

bool nvControl::SetIoCtrl(int value)
{



}

void nvControl::SetAtmmOpenPos2(ScanParam param, int index, int iCollimatorStep, int iBowtieStepMax, int iBowtieStepMin)
{




















}

std::string nvControl::NVLogFormat(const char *fmt, ...)
{












}

std::string nvControl::GetFileName(int index)
{























}

bool nvControl::SetMotorCollimatorZ(int iMotorNum, int iCoverPos)
{















}

bool nvControl::SetMotorBowtieStatus(int iMotorNum, bool isEnable)
{





















}

void nvControl::SetAtmmOpenPos_CollimatorZ(int index, int iCollimatorStep)
{






}

void nvControl::SetAtmmOpenPos_Bowtie(int index, int iBowtieStep)
{






}

void nvControl::SetAtmmOpenPos(ScanParam &param)
{


#if 1




























#endif

}

void nvControl::SetAtmmOpenPos(AecAtmmParam stAecAtmmParam, bool isMoveCollimatorZ, bool isMoveBowtie)
{































}

bool nvControl::SetFOV(int value)
{



















































}

bool nvControl::SetCollimatorZ(int value) // 0:close, 1:1/4, 2:2/4, 3:3/4, 4:4/4open
{












































}

bool nvControl::SetScanParam()
{





}

bool nvControl::SetScanParam(const ScanParam &param)
{
















































































































}

bool nvControl::SetCollimatorZ(int sourceNum, int value)
{









}

bool nvControl::SetBowtieEnabled(int sourceNum, bool value)
{









}

bool nvControl::SetDevReadyStatus(int value)
{


}

bool nvControl::GetDevReadyStatus(int &value)
{


}

bool nvControl::GetDevOnlineStatus(int &value)
{


}

bool nvControl::SetPowerOn(bool on)
{


}

bool nvControl::GetCtboxVersion(int &value)
{



}

bool nvControl::GetIfboxVersion(int &value)
{



}

bool nvControl::GetTubeVersion(int &value)
{



}

bool nvControl::GetDetectorVersion(int &value)
{



}

bool nvControl::GetMotionVersion(int &value)
{



}

bool nvControl::GetPduVersion(int &value)
{



}

bool nvControl::GetTubeIntfVersion(const int index, int &value)
{



}

bool nvControl::GetImgProcVersion(const int index, int &value)
{



}

bool nvControl::GetBedSoftVersion(int &value)
{



}

bool nvControl::GetBoxSoftVersion(int &value)
{



}

bool nvControl::GetPanelSoftVersion(int &value)
{



}

bool nvControl::GetImgProcTemp(const int imgProcIndex, const int chipIndex, const int tempIndex,

{






}

bool nvControl::SetDualEnergyEnable(bool bEnable)
{


}

bool nvControl::SetInjectorEnable(bool bEnable)
{


}

bool nvControl::GetInjectorReady(bool &value)
{



}

bool nvControl::GetInjectorAction(bool &value)
{



}

bool nvControl::SetInjectorDelay(int value)
{


}

bool nvControl::GetDevStatus(int &nDoorValue, int &nEmerStop)
{



}

bool nvControl::GetMotionStatus(bool bRequireMore)
{



















































}

bool nvControl::GetM15PostInfo()
{



























}

bool nvControl::GetM15PostKV(int sourceGroup, int sourceId, int &value)
{



}

bool nvControl::GetM15PostTime(int sourceGroup, int sourceId, int &value)
{



}

bool nvControl::GetM15PostMAS(int sourceGroup, int sourceId, int &value)
{



}

bool nvControl::GetM15PercentOfHeat(int sourceGroup, int sourceId, int &value)
{



}

bool nvControl::GetOilTemperature(int sourceGroup, int sourceId, int &value)
{



}

bool nvControl::GetPercentOfTubeHeat(int sourceGroup, int sourceId, int &value)
{



}

bool nvControl::SetWorkMode(int value)
{



}

bool nvControl::SetSingleThreeSpotCycleMode(int value)
{


}

bool nvControl::SetSingleThreeSpotCycleTimes(int value)
{


}

bool nvControl::SetDynamicGain(int value)
{



}

bool nvControl::SetPostOffsetNum(int value)
{


}

bool nvControl::GetIfBoxStatus(int &value)
{


}

bool nvControl::SetDetectorWorkReg(int regValue)
{


}

bool nvControl::GetDetectorWorkReg(int &regValue)
{



}

bool nvControl::SetifBoxMultiFunCtl(int value)
{


}

bool nvControl::GetifBoxMultiFunCtl(int &value)
{



}

bool nvControl::SetInjectPotion(bool value)
{


}

bool nvControl::GetInjectPotion(bool &value)
{


}

bool nvControl::EnterWorkMode()
{











}

bool nvControl::LeaveWorkMode()
{














}

bool nvControl::SetCoolingDisabled()
{


}

bool nvControl::SetTargetTemperature(int value) // 实际温度的10倍
{
}

bool nvControl::GetTargetTemperature(int &value) // 实际温度的10倍
{



}

void nvControl::GetScanParam(map<string, string> &scanParam)
{
}



bool nvControl::SetExposureCtrl(bool exposureCtrl)
{

}


bool nvControl::SetValueInt32(uint regAddr, int value)
{

}

bool nvControl::GetValueInt32(uint regAddr, int &value)
{

}