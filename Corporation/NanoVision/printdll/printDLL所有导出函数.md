C#界面按钮布局									lbPageCount
1加载 2添加3*3 3添加4*4 4打开序列 6添加序列A 7打印序列B 13打印  14当前页	5重置
                    8：1 9：2 10：3 11：4 12：5 
16重排版 17设置模式 18定位线							15 删除


1加载
extern "C" __declspec(dllexport) bool showDialog( HWND parent, int pWidth , int pHeight , int pX , int pY )

FormClosing
15 删除
extern "C" __declspec(dllexport) bool destroyDialog()

14当前页
extern "C" __declspec(dllexport) int  f_GetPrintPage()

2添加3*3    3添加4*4
extern "C" __declspec(dllexport) int f_AddPrintPage(int pRow , int pCol , int pID )

13打印
extern "C" __declspec(dllexport) int f_PrintDicomImage(int pPage ,char pFilePath,char pFilleNmae)

6添加序列A f_AddPrintPageBySeries(5, 5, 0);同时获取页数，放到lb_PageCount.Text中
7打印序列B f_AddPrintPageBySeries(5, 5, 1);同时获取页数，放到lb_PageCount.Text中
extern "C" __declspec(dllexport) int f_AddPrintPageBySeries(int pRow , int pCol , int pSeriesID )

4打开序列 创建按钮 mXButton.Text = "序列N"; 放在flowLayoutPanel1.Controls中
extern "C" __declspec(dllexport) int f_LoadDicomSeries(char msg )

 mXButton.Text = "序列N" f_SetImageByID(0, 0, 0, 0, 0, 0);
extern "C" __declspec(dllexport) int f_SetImageByID(int pPage ,int pRow, int pCol, int pSeries, int pImageNo,int pMode)

6添加序列A，同时获取页数，放到lb_PageCount.Text中
7打印序列B，同时获取页数，放到lb_PageCount.Text中
extern "C" __declspec(dllexport) int f_GetPageCount()

8：1 f_SetDisplayPage(0);
9：2 f_SetDisplayPage(1);
10：3 f_SetDisplayPage(2);
11：4 f_SetDisplayPage(3);
12：5  f_SetDisplayPage(4);
extern "C" __declspec(dllexport) int f_SetDisplayPage(int pPage)

5重置
extern "C" __declspec(dllexport) int f_Reset()

16重排版
extern "C" __declspec(dllexport) int f_Format(int pRow , int pCol)

17设置模式 在 true false之间切换
extern "C" __declspec(dllexport) int f_SetWindowColorAndLevelMode(bool pAllMode)

18定位线
extern "C" __declspec(dllexport) int f_SetLocationLineMode(bool pOpened)


extern "C" __declspec(dllexport) long f_Ver()
