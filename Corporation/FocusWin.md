延锋伟世通
管理员
.\\ctcit
Yfvesfudian1@

qliu9
Money2005

宜硕
192.168.1.1
fgadmin fgadmin


focuswin focusguest 1a2b3C4d
focuswinboss 1a2b3C4d5
1234*.com
xiaomi_robot
1234.com
OpenCR
https://item.taobao.com/item.htm?id=545389984583
以下的板子跟上一个板子画到同一个板子上

升降杆控制板
https://detail.tmall.com/item.htm?id=559165121504
IPAd充电驱动
https://detail.tmall.com/item.htm?id=536770526104
电源板
https://item.taobao.com/item.htm?id=525275474884

工控机天线
https://item.taobao.com/item.htm?id=564720021539




1.action lib
2.dynamixel servo
3.去掉imu
4.


吸气：指数曲线上升，该过程需要1.5S
呼气：指数曲线下降，该过程需要1.5S.
对成人而言，平均每分钟呼吸 16~18次；
对儿童而言，平均每分钟呼吸20次；


八字型：
uint8_t index[]={
1,1,2,2,3,4,6,8,10,14,19,25,33,44,59,80,107,143,191,255}



正玄波：
char BL[242]=  //注意这个数组类型，int 和 char 会有不同的效果
            {0,1,3,4,6,8,9,11,13,14,16,18,19,21,23,24,26,28,29,31,33,34,36,
              38,39,41,43,44,46,47,49,51,52,54,56,57,59,60,62,64,65,67,69,70,
              72,73,75,76,78,80,81,83,84,86,88,89,91,92,94,95,97,98,100,101,
              103,104,106,107,109,111,112,113,115,116,118,119,121,122,124,
              125,127,128,130,131,132,134,135,137,138,139,141,142,144,145,
              146,148,149,150,152,153,154,156,157,158,160,161,162,164,165,
              166,167,169,170,171,172,174,175,176,177,178,180,181,182,183,
              184,185,186,188,189,190,191,192,193,194,195,196,197,199,200,
              201,202,203,204,205,206,207,208,209,210,210,211,212,213,214,
              215,216,217,218,219,219,220,221,222,223,224,224,225,226,227,
              227,228,229,230,230,231,232,233,233,234,235,235,236,236,237,
              238,238,239,240,240,241,241,242,242,243,243,244,244,245,245,
              246,246,247,247,247,248,248,249,249,249,250,250,250,251,251,
              251,252,252,252,252,253,253,253,253,254,254,254,254,254,254,
              255,255,255,255,255,255,255,255,255,255,255,255,255};

void setup()
{
}

void loop() {
   int i=0,s=2,t=1;;
   while (1)
   {
      analogWrite(3,BL[i]);
      i+=t;  //i = i + t;
      if (i > 240) t = -1; //到达上限变减
      if (i < 1) t = 1; //到达下限变加
      delay(s);
   }
}