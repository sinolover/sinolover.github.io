vtk  9.0
itk  5.0
python 3.6.8
对象之间关系
vtkConeSource-》vtkPolyDataMapper
vtkRenderer-》vtkRenderWindow-》vtkRenderWindowInteractor

vtkRenderWindowInteractor->SetRenderWindow(vtkRenderWindow);
vtkRenderWindow->AddRenderer(vtkRenderer);
vtkActor-》SetMapper（vtkPolyDataMapper）
vtkRenderer->AddActor（vtkActor）
vtkRenderWindow->SetInteractorStyle(vtkInteractorStyleTrackballCamera);
vtkRenderWindow->Render();
********************************************************
在VTK中vtkExtractVOI类实现由用户指定的区域范围提取图像的子图像。该Filter的输入和输出都是一个vtkImageData
********************************************************
 itk和vtk存储图像时的起点不一样，一个在左下角，一个在左上角，所以在将vtk的图像转化为itk的时，需要进行坐标的转换
  ImageToVTKImageFilter
 vtkImageToImageFilter
 VTKImageToImageFilter  可用
********************************************************
vtkStandardNewMacro是VTK中定义的一个宏，作用是定义一个通用的New()函数 ，使用时需要将实际的类名传递给它。

三维空间中渲染对象最常用的vtkProp子类是vtkActor(表达场景中的几何数据)和vtkVolume(表达场景中的体数据)；
二维空间中的数据则是用vtkActor2D表达。
Prop依赖于两个对象，一个是Mapper(vtkMapper)对象，负责存放数据和渲染信息，另一个是属性(vtkProperty)对象，负责控制颜色、不透明度等参数。
vtkPieChartActor(用于创建数组数据的饼图可视化表达)。
vtkProp子类负责确定渲染场景中对象的位置、大小和方向信息。
tkImageActor(负责图像显示)
vtkPieChartActor(用于创建数组数据的饼图可视化表达)
vtkInteractorStyle等监听这些消息并进行处理以完成旋转、拉伸和放缩等运动控制。
vtkRenderWindowInteractor 提供平台独立的响应鼠标、键盘和时钟事件的交互机制
vtkInteractorStyleImage 主要用于显示二维图像时的交互

vtkOutlineFilter  获取数据的边框
vtkImageClip  对数据进行剪切
vtkDataArray::GetDataTypeSize 获取类型占用的空间
vtkImageExtractComponents 将数据中的指定Component抽取出来组成新的数据体
vtkImageCast 对数据类型进行强制转换
vtkImageShiftScale 对数据类型进行按比例转换
vtkImageMagnitude 计算图像的梯度
vtkImageExtractComponent来提取每个方向的梯度分量进行显示。（彩色图像要转换为灰度图像才可计算梯度）
itk::VesselTubeSpatialObject源自于itk::TubeSpatialObject。它表示从一幅图像分割而得到的一个血管。一个VesselTubeSpatialObject是以一系列点来描述的，每个点都具有一个位置、一个半径和法线。
********************************************************
GetClassName()可以得到VTK对象的类型
********************************************************
vtkLight常的方法有：
SetColor() — 设置光照的颜色，以RGB的形式指定颜色。
SetPosition() — 设置光照位置。
SetFocalPoint() — 设置光照焦点。
SetIntensity() — 设置光照的强度。
SetSwitch() / SwitchOn()/ SwitchOff()— 打开或关闭对应的光照。
SetPositional() / GetPositional()/ PositionalOn()/PositionalOff()一类方法来设置位置光照。
********************************************************
vtkCamera
相机位置：即相机所在的位置，用方法vtkCamera::SetPosition()设置。
相机焦点：用方法vtkCamera::SetFocusPoint()设置，默认的焦点位置在世界坐标系的原点。
SetClippingRange()；SetFocalPoint()；SetPosition()分别设置相机的前后裁剪平面，焦点和位置。
ComputeViewPlaneNormal()方法是根据设置的相机位置、焦点等信息，重新计算视平面(View Plane)的法向量。
SetViewUp()方法用于设置相机朝上方向。
最后用方法vtkRenderer::SetActiveCamera()把相机设置到渲染场景中。
********************************************************
vtkCylinderSource
vtkCylinderSource::SetHeight() ——设置柱体的高。
vtkCylinderSource::SetRadius() ——设置柱体横截面的半径。
vtkCylinderSource::SetResolution() ——设置柱体横截面的等边多边形的边数。转动一下柱体，然后数数柱体横截面有多少条边，应该就能明白这个参数表示什么意思。
********************************************************
vtkVolumeProperty
volumeProperty1->SetAmbient(0.4);//设置环境温度系数
volumeProperty1->SetDiffuse(0.6);//设置漫反射系数
volumeProperty1->SetSpecular(0.2);//设置镜面反射系数
********************************************************
vtkMarchingCubes
void SetValue (int i, double value) 设置第i个等值面的值为value
void SetNumberOfContours (int number) 设置等值面的个数
********************************************************
vtkActor
//皮肤颜色
m_actor->GetProperty()->SetDiffuseColor(0, .49, .25);
//设置反射率
m_actor->GetProperty()->SetSpecular(0.3); 
//设置反射光强
m_actor->GetProperty()->SetSpecularPower(20);
//不透明度
m_actor->GetProperty()->SetOpacity(1);
Actor其中的一个功能就是负责将模型从Model坐标系统变换到World坐标系统。
********************************************************
Model坐标系统是定义模型时所采用的坐标系统，通常是局部的笛卡尔坐标系。例如，我们要定义一个表示球体的Actor，一般的做法是将该球体定义在一个柱坐标系统里。
World坐标系统是放置Actor的三维空间坐标系，Actor其中的一个功能就是负责将模型从Model坐标系统变换到World坐标系统。每一个模型可以定义有自己的Model坐标系统，但World坐标系只有一个，每一个Actor必须通过放缩、旋转、平移等操作将Model坐标系变换到World坐标系。World坐标系同时也是相机和光照所在的坐标系统。
View坐标系统表示的是相机所看见的坐标系统。X、Y、Z轴取值为[-1, 1]，X、Y值表示像平面上的位置，Z值表示到相机的距离。相机负责将World坐标系变换到View坐标系。
********************************************************
渲染引擎主要负责数据的可视化表达，

可视化管线是指用于获取或创建数据，处理数据，以及把数据写入文件或者把数据传递给渲染引擎进行显示，
这样的一种结构在VTK里就称之为可视化管线。数据对象(Data Object)、处理对象(Process Object)和
数据流方向(Direction of Data Flow)是可视化管线的三个基本要素。每个VTK程序都会有可视化管线存在。
********************************************************
vtkDataSet的组织结构由拓扑结构(Topology)和几何结构(Geometry)两部分组成。
拓扑结构描述了物体的构成形式，几何结构描述了物体的空间位置关系。
换言之，点数据(Point Data)所定义的一系列坐标点构成了vtkDataSet(数据集)的几何结构；
点数据的连接(点的连接先形成单元数据(Cell Data)，
由单元数据再形成拓扑)就形成了数据集的拓扑结构。
********************************************************
VTK中vtkDijkstraGraphGeodesicPath采用了上述算法，可以用于测量三维物体的测地距离
ITK 血管空间对象 VesselTubeSpatialObject
********************************************************
点云配准算法对比
可以得出，配准精度最高的是3Dsc，但耗时最长。
耗时：ndt<fpfh<pfh<3dsc
********************************************************
vtkDelaunay3D剖分效果不错，但是输出的是vtkUnstructuredGrid类，不能用vtkCurvatures来计算曲率。vtkDelaunay2D输出的是vtkPolydata类，可以用vtkCurvatures来计算曲率
********************************************************
其中一个最重要的特征是vtkDataObject可以在可视化管道中处理。vtkDataSet 和派生类用于科学可视化；vtkPolyData用于表示多边形网格; vtkUnstructuredGrid表示网格；vtkImageData表示2D和3D像素和体素数据。
管道体系结构由三个基本类对象组成：表示数据的对象（vtkDataObject），、将数据对象从一种形式映射到另一种形式的对象（vtkAlgorithm）、对象执行的一个管道（vtkExecutive）
********************************************************
vtk模型简化vtkDecimatePro和vtkQuadricDecimation
********************************************************
itk提供了两个的补洞类：
itkGrayscaleFillholeImageFilter（针对灰度图像）, itkBinaryFillholeImageFilter（针对二值图像）