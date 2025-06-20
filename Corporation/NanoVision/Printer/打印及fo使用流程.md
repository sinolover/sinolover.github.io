# 打印及fo使用流程
一、Wrapper使用流程
1. 引用dll 
Sinogram.Pet.ImageViewer.Interop.dll
Sinogram.Pet.ImageViewer.Winform.dll

2.创建winformHost 
3.创建ImageViewerWrapper，指定width height。并加入winFormHost中
4.创建需要数量的imageViewerWrapper.CreateSingleSlice();
5.加载dicom图像loaddicomfile
6.SetViewData
7.SetWWWWL(必须在SetViewData后面)/SetRation/SetZoom...





//
二、打印流程
1.创建打印Job 
  printJob=new PrintJob() 
  初始化参数：
  a.IP Port CallingAE CalledAE 
  b.MediuType=“PAPER” 
  c.NumberOfCopies=1 
  d.FilmSessionLable
  内部完成：
  a.创建 FilmSession （使用了1中的b c d进行参数初始化）

2.创建FilmBox
  printJob.StartFilmBox(0
  参数：
  a.排版格式：Standard\\*.* ROWS COLS XCC
  b.纸张方向：Portrait Orientation
  c.纸张大小：A4 A3 ...
  d.MagnificationType
  e.BorderDesity 边框颜色
  f.EmptyImageDesity 空白图片颜色
  内部完成：
  a.FilmSession
3.加入要打印的图片（循环加入多个）
  a.使用new DicomImage(@"DicomFilePath").RenderImage().As<Bitmap>() 生成Bitmap
  b.printJob.AddImage(Bitmap)
4.完成Filmbox
5.打印
  await printJob.Print()










  
