# Dicom文件基本操作
## Dicom文件基本操作

`var file = DicomFile.Open(@"test.dcm"); // 打开文件`  
`var file = await DicomFile.OpenAsync(@"test.dcm"); // 异步打开`

file中保存了dicomFile信息。

`var dataSet =file.Dataset(); // dataSet中保存的是dcm的基本数据，标签信息及piexldata信息。`

读取标签  
`var patientid = file.Dataset.Get<string>(DicomTag.PatientID);`

添加并修改标签  
`file.Dataset.Add(DicomTag.PatientsName, "DOE^JOHN");`

改变TransferSyntax属性，用图像数据的压缩等。  
`file = file.ChangeTransferSyntax(DicomTransferSyntax.JPEGProcess14SV1);`

`file.Save(@"output.dcm"); // 保存文件到本地`  
`file.Dataset.Remove(DicomTag.PixelData); 删除标签`

## 图像操作

图像数据存贮在PixelData中，根据其DicomTag.NumberOfFrames帧数的设置，可知其有多少帧图像数据。获取第一帧图像数据可用  
`var pixel = DicomPixelData.Create(file.Dataset);`  
`var frame = pixel.GetFrame(0);`得到第一帧数据，可在此数据中进行设置，或者这样idx第几帧  
`var header = DicomPixelData.Create(dataset);`  
`var pixelData = PixelDataFactory.Create(header, idx);`

## 图像压缩解决方案

可使用 `file = file.ChangeTransferSyntax(DicomTransferSyntax.ExplicitVRLittleEndian);`  
进行解压缩。  
然后在设置 `file = file.ChangeTransferSyntax(DicomTransferSyntax.JPEG2000Lossless);`进行压缩。

## 图像显示
~~~c#
var image = new DicomImage(@"test.dcm");
image.RenderImage().AsBitmap().Save(@"test.jpg");
ImageManager.SetImplementation(WPFImageManager.Instance);
(WriteableBitmap) image.RenderImage().AsWriteableBitmap(); |
~~~