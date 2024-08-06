​

 转自：[c# - C#用fo-dicom对CT图像的PixelData进行处理和转换 - IT工具网](https://www.coder.work/article/2897927 "c# - C#用fo-dicom对CT图像的PixelData进行处理和转换 - IT工具网")

对于某些测试，我试图操纵 `PixelData` 来处理 dicom 格式存储的 CT 图像的元素，并使用 C# 中的 Fellow Oak Dicom 将其写回文件中。经过一番研究，我发现我想要处理的矩阵在 `Buffer` 中。的 `PixelData`存储在 `byte` -大批。所以我写了以下代码:
~~~csharp
DicomFile ctFile = DicomFile.Open(image);
var pixDat = ctFile.Dataset.Get<byte[]>(DicomTag.PixelData);

for (int i = 0; i < pixData.Length; i++)
{
    pixDat[i] = Convert.ToByte(200);
}

ctFile.Dataset.AddOrUpdate<byte[]>(DicomTag.PixelData, pixDat);
ctFile.Save("new file folder");` 
~~~
  
这是我的第一次尝试，我收到了 `Exception`在 `AddOrUpdate`命令，因为它无法转换 `byte` -数组到OB。  
阅读Pianykh 的关于 DICOM 的书，OB 表示其他字节字符串。但到目前为止我无法转换 `byte` -数组到OB。当我尝试这个代码片段时:
~~~csharp
DicomOtherByte dob = new DicomOtherByte(DicomTag.PixelData, pixDat);
ctFile.Dataset.AddOrUpdate<DicomOtherByte>(DicomTag.PixelData, dob);` 
~~~
`Exception`仍然在调用 `AddOrUpdate`因为无法将项目转换为 OB。在 stackoverflow、git 或 google 中的 fo-dicom 文档中搜索我仍然不知道如何处理它。所以我想知道如何将我的操纵矩阵转换为 OB，因为我认为 `DicomOtherByte`是OB。  
  
编辑:`Exception`是“无法使用 Dicom.DicomOtherByte 类型的值创建 OB 类型的 DICOM 元素” - System.InvalidOperationException  
  
提前致谢。

**最佳答案**

Dicom 数据集中的像素数据非常特别。它不能作为单个标签轻松读取或写入。 Fo-Dicom 具有处理像素数据的特殊函数和类。  
  
下面是一个例子:
~~~csharp
`DicomFile ctFile = DicomFile.Open(@"C:\Temp\original.dcm");

// Create PixelData object to represent pixel data in dataset
DicomPixelData pixelData = DicomPixelData.Create(ctFile.Dataset);
// Get Raw Data
byte[] originalRawBytes = pixelData.GetFrame(0).Data;

// Create new array with modified data
byte[] modifiedRawBytes = new byte[originalRawBytes.Length];
for (int i = 0; i < originalRawBytes.Length; i++)
{
    modifiedRawBytes[i] = (byte)(originalRawBytes[i] + 100);
}

// Create new buffer supporting IByteBuffer to contain the modified data
MemoryByteBuffer modified = new MemoryByteBuffer(modifiedRawBytes);

// Write back modified pixel data
ctFile.Dataset.AddOrUpdatePixelData(DicomVR.OB, modified);

ctFile.Save(@"C:\Temp\Modified.dcm");` 
~~~
  
请注意，有更多的辅助类可以直接以特定格式处理像素数据，例如 `PixelDataConverter`和 `PixelDataFactory` .  
  
此外，如果您想使用实际图像，请使用 `DicomImage`类(class)。

`DicomImage image = new DicomImage(ctFile.Dataset);` 

关于c# - C#用fo-dicom对CT图像的PixelData进行处理和转换，我们在Stack Overflow上找到一个类似的问题： [type conversion - Manipulating and Converting PixelData of CT Image with fo-dicom in C# - Stack Overflow](https://stackoverflow.com/questions/52292288/ "type conversion - Manipulating and Converting PixelData of CT Image with fo-dicom in C# - Stack Overflow")

​