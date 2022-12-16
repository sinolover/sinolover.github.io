直接上代码：
~~~csharp
DicomFile dcmFile = DicomFile.Open(filename);
var name = dcmFile.Dataset.GetString(DicomTag.PatientName);
ushort bitStore = dcmFile.Dataset.GetSingleValue<ushort>(DicomTag.BitsStored);
ushort bitAlloced = dcmFile.Dataset.GetSingleValue<ushort>(DicomTag.BitsAllocated);
ushort simplePerPixel = dcmFile.Dataset.GetSingleValue<ushort>(DicomTag.SamplesPerPixel);
ushort width = dcmFile.Dataset.GetSingleValue<ushort>(DicomTag.Columns);
ushort height = dcmFile.Dataset.GetSingleValue<ushort>(DicomTag.Rows);
int windowCenter = dcmFile.Dataset.GetSingleValue<int>(DicomTag.WindowCenter);
int windowWidth = dcmFile.Dataset.GetSingleValue<int>(DicomTag.WindowWidth);
string modality = dcmFile.Dataset.GetString(DicomTag.Modality);
var gdiImg = new Bitmap(width, height);
if (bitStore > 8)
{
    ushort[] datass = dcmFile.Dataset.GetValues<ushort>(DicomTag.PixelData);
    ushort maxVal = datass.Max();
    ushort minVal = datass.Min();
    double compressFactor = byte.MaxValue / (double)(maxVal - minVal) - 0.00000000001;
    if (simplePerPixel == 1) // gray
    {
        System.Drawing.Color c = System.Drawing.Color.Empty;
        for (int i = 0; i < height; i++)
        {
            for (int j = 0; j < width; j++)
            {
                int grayGDI;
                double gray = datass[i*width + j];
                //int grayStart = (windowCenter - windowWidth / 2);
                //int grayEnd = (windowCenter + windowWidth / 2);
                //if (gray < grayStart)
                //{
                //    grayGDI = 0;
                //}
                //else if (gray > grayEnd)
                //{
                //    grayGDI = 255;
                //}
                //else
                //{
                //    grayGDI = (int)((gray - grayStart) * 255 / windowWidth);
                //}
                //if (grayGDI > 255)
                //{
                //    grayGDI = 255;
                //}
                //else if (grayGDI < 0)
                //{
                //    grayGDI = 0;
                //}
                //if (modality == "CT")
                //{
                //    grayGDI = 255 - grayGDI;
                //}

                grayGDI = (int)(gray*compressFactor);
                c = System.Drawing.Color.FromArgb(grayGDI, grayGDI, grayGDI);
                gdiImg.SetPixel(j,i,c);
            }
        }
        gdiImg.Save(@"e:\dicom2bmp.bmp", System.Drawing.Imaging.ImageFormat.Bmp);
    }
    else if (simplePerPixel == 3) //RGB
    {
        //c = Color.FromArgb(pixData[0], pixData[1], pixData[2]);
    }
}
else //(bitStore <= 8)
{
    byte[] datas = dcmFile.Dataset.GetValues<byte>(DicomTag.PixelData);
    if (simplePerPixel == 1) // gray
    {

    }
    else if (simplePerPixel == 3) //RGB
    {

    }
}
~~~
代码只实现了灰度图像转换（图像存储为OW，OB的代码基本相同），彩色图像转换更简单。
目前代码存在的问题是转为DICOM图像后失真较严重，和直接查看DICOM文件有一定的差别。仅供学习参考。
同时图像存储的TAG不仅有7FE0,0010，还有7FE0,0008和7FE0,0009，这两种格式存储的DICOM文件暂时还没有。关于DICOM标准的研究也是在逐步进行中。
fo-dicom是一个跨平台的DICOM开源库，目前支持Android，iOS还有电脑。是值得学习的一个开源库。