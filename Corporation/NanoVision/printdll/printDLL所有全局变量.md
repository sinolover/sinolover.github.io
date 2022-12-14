【Main.cpp】
~~~cpp
VTK_MODULE_INIT(vtkRenderingOpenGL2)
VTK_MODULE_INIT(vtkRenderingVolumeOpenGL2)
VTK_MODULE_INIT(vtkInteractionStyle)
VTK_MODULE_INIT(vtkRenderingFreeType)


QPointer<QWinWidget> win;
QPointer<QGridLayout> vMainLayout_Main;
QPointer<QTabWidget> gMainTabPage;
QPointer<QHBoxLayout> mMainWidget;
vtkNew<vtkCamera> mMainCamera;
vtkNew<vtkLookupTable> mMainLookupTable;
QMap<QString, QPointer<uFunBase> > mFunBaseMap;
vtkNew<uEmptyImageData> muEmptyImageData;
QString mCurButtonName = "";
QString mCurUID = "";
uInitMenu mInitMenu = new uInitMenu();
vector<uWidget2d> gVectorWidget2d;

int mCountItem = 0;
int mRow = 0;
int mCol = 0;

int mDICOMReaderCount=0;
int mEnabled=0;

uLocationLine gLocationLine;
uDisaplayPage gDisaplayPage;

vtkSmartPointer< vtkStringArray > mStringArray				打开Dicom序列的所有文件的文件名
uDicomSeries mDicomSeries =0;						存放Dicom序列
~~~