# c#数字图像处理（二）彩色图像灰度化，灰度图像二值化
为加快处理速度,在图像处理算法中,往往需要把彩色图像转换为灰度图像,在灰度图像上得到验证的算法,很容易移  
植到彩色图像上。  
24位彩色图像每个像素用3个字节表示,每个字节对应着R、G、B分量的亮度(红、绿、蓝)。当R、G、B分量值不同时,表现为彩色图像;当R、G、B分量值相同时,表现为灰度图像,该值就是我们所求的一般来说,转换公式有3种。第一种转换公式为:

Gray(i,j)=\[R(i,j)+G(i,j)+B(i,j)\]÷3　　　　　　　　　　　　(2.1)

其中,Gray(i,j)为转换后的灰度图像在(i,j)点处的灰度值。该方法虽然简单,但人眼对颜色的感应是不同的,因此有了第二种转换公式:

Gray(i,j)=0299R(i,j)+0.587×G(i,j)+0.114×B(i,j)　　　　　　(2.2)

观察上式,发现绿色所占的比重最大,所以转换时可以直接使用G值作为转换后的灰度

Gray(i,j)=G(i,j)　　　　　　　　　　　　　　　　　　　　(2.3)

在这里,我们应用最常用的公式(2.2),并且变换后的灰度图像仍然用24位图像表示。

1.提取像素法

这种方法简单易懂,但相当耗时,完全不可取.  
该方法使用的是GD+中的 Bitmap Getpixel和 BitmapSetpixel.方法。为了将位图的颜色设置为灰度或其他颜色,就需要使用 Gepiⅸxel来读取当前像素的颜色,再计算灰度值,最后使用 Setpixel来应用新的颜色。双击“提取像素法” Button控件,为该控件添加 Click事件,

代码如下:
~~~csharp 
/// <summary>
/// 提取像素法
/// </summary>
private void pixel_Click(object sender, EventArgs e)
{
    if (curBitmpap != null)
    {
         Color curColor; int ret; //二维图像数组循环
         for(int i = 0; i < curBitmpap.Width; i++)
         {
             for(int j = 0; j < curBitmpap.Height; j++)
             { //获取该像素点的RGB颜色值
                  curColor = curBitmpap.GetPixel(i, j); //利用公式计算灰度值
                  ret = (int)(curColor.R * 0.299 \+ curColor.G * 0.587 \+ curColor.B * 0.114); //设置该像素点的灰度值，R=G=B=ret
 curBitmpap.SetPixel(i, j, Color.FromArgb(ret, ret, ret));
              }
         }//对窗体进行重新绘制，这将强制执行Paint事件处理程序
         Invalidate();
    }
}
~~~

2.内存法  
该方法就是把图像数据直接复制到内存中,这样就使程序的运行速度大大提高。双击“内存法”按钮控件,为该控件添加Cick事件,代码如下:
~~~csharp
　　　　 /// <summary>
        /// 内存法(适用于任意大小的24位彩色图像)
        /// </summary>
        private void memory_Click(object sender, EventArgs e)
        {
            if (curBitmpap != null)
            {
                //位图矩形
                Rectangle rect = new Rectangle(0, 0, curBitmpap.Width, curBitmpap.Height);
                //以可读写的方式锁定全部位图像素
                System.Drawing.Imaging.BitmapData bmpData = curBitmpap.LockBits(rect, System.Drawing.Imaging.ImageLockMode.ReadWrite, curBitmpap.PixelFormat);
                //得到首地址
                IntPtr ptr = bmpData.Scan0;

                //定义被锁定的数组大小，由位图数据与未用空间组成的
                int bytes = bmpData.Stride * bmpData.Height;
                //定义位图数组
                byte[] rgbValues = new byte[bytes];
                //复制被锁定的位图像素值到该数组内
                System.Runtime.InteropServices.Marshal.Copy(ptr, rgbValues, 0, bytes);

                //灰度化
                double colorTemp = 0;
                for (int i = 0; i < bmpData.Height; i++)
                {
                    //只处理每行中是图像像素的数据，舍弃未用空间
                    for (int j = 0; j < bmpData.Width * 3; j += 3)
                    {
                        //利用公式计算灰度值
                        colorTemp = rgbValues[i * bmpData.Stride + j + 2] * 0.299 + rgbValues[i * bmpData.Stride + j + 1] * 0.587 + rgbValues[i * bmpData.Stride + j] * 0.114;
                        //R=G=B
                        rgbValues[i * bmpData.Stride + j] = rgbValues[i * bmpData.Stride + j + 1] = rgbValues[i * bmpData.Stride + j + 2] = (byte)colorTemp;
                    }
                }

                //把数组复制回位图
                System.Runtime.InteropServices.Marshal.Copy(rgbValues, 0, ptr, bytes);
                //解锁位图像素
                curBitmpap.UnlockBits(bmpData);//对窗体进行重新绘制，这将强制执行Paint事件处理程序
                Invalidate();
            }
        }
~~~

3.指针法 
该方法与内存法相似,开始都是通过 Lockbits方法来获取位图的首地址。但该方法更简洁,直接应用指针对位图进行操作。

为了保持类型安全,在默认情况下,C#是不支持指针运算的,因为使用指针会带来相关的风险。所以C#只允许在特别标记的代码块中使用指针。通过使用 unsafe关键字,可以定义可使用指针的不安全上下文。

双击“指针法”按钮控件,为该控件添加 Click事件,代码如下:
~~~csharp
/// <summary>
/// 指针法
/// </summary>
        private void pointer_Click(object sender, EventArgs e)
        {
            if (curBitmpap != null)
            {
                //位图矩形
                Rectangle rect = new Rectangle(0, 0, curBitmpap.Width, curBitmpap.Height);
                //以可读写的方式锁定全部位图像素
                System.Drawing.Imaging.BitmapData bmpData = curBitmpap.LockBits(rect, System.Drawing.Imaging.ImageLockMode.ReadWrite, curBitmpap.PixelFormat);

                byte temp = 0;
                //启用不安全模式
                unsafe
                {
                    //得到首地址
                    byte* ptr = (byte*)(bmpData.Scan0);
                    //二维图像循环
                    for (int i = 0; i < bmpData.Height; i++)
                    {
                        for (int j = 0; j < bmpData.Width; j++)
                        {
                            //利用公式计算灰度值
                            temp = (byte)(0.299 * ptr[2] + 0.587 * ptr[1] + 0.114 * ptr[0]);
                            //R=G=B
                            ptr[0] = ptr[1] = ptr[2] = temp;
                            //指向下一个像素
                            ptr += 3;
                        }
                        //指向下一行数组的首个字节
                        ptr += bmpData.Stride - bmpData.Width * 3;
                    }
                }
                //解锁位图像素
                curBitmpap.UnlockBits(bmpData);//对窗体进行重新绘制，这将强制执行Paint事件处理程序
                Invalidate();
            }
        }~~~

由于启动了不安全模式,为了能够顺利地编译该段代码,必须设置相关选项。在主菜单中选择“项目|gray属性”,在打开的属性页中选择“生成”属性页,最后选中“允许不安全代码”复选框。

三种方法的比较

从3段代码的长度和难易程度来看,提取像素法又短又简单。它直接应用GD+中的Bitmap. Getpixel方法和 Bitmap. Setpixel方法,大大减少了代码的长度,降低了使用者的难度,并且可读性好。但衡量程序好坏的标准不是仅仅看它的长度和难易度,而是要看它的效率,尤其是像图像处理这种往往需要处理二维数据的大信息量的应用领域,就更需要考虑效率了为了比较这3种方法的效率,我们对其进行计时。首先在主窗体内添加一个 Label控件和 Textbox控件。

添加一个类：HiPerfTimer

~~~csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Runtime.InteropServices;
using System.ComponentModel;
using System.Threading;

namespace gray
{
    internal class HiPerfTimer
    {
        //引用win32API中的QueryPerformanceCounter()方法
        //该方法用来查询任意时刻高精度计数器的实际值
        [DllImport("Kernel32.dll")]         //using System.Runtime.InteropServices;
        private static extern bool QueryPerformanceCounter(out long lpPerformanceCount);

        //引用win32API中的QueryPerformanceCounter()方法
        //该方法用来查询任意时刻高精度计数器的实际值
        [DllImport("Kernel32.dll")]
        private static extern bool QueryPerformanceFrequency(out long lpFrequency);

        private long startTime, stopTime;
        private long freq;

        public HiPerfTimer()
        {
            startTime = 0;
            stopTime = 0;

            if(QueryPerformanceFrequency(out freq) == false)
            {
                //不支持高性能计时器
                throw new Win32Exception();     //using System.ComponentModel;
            }
        }

        //开始计时
        public void Start()
        {
            //让等待线程工作
            Thread.Sleep(0);                //using System.Threading;

            QueryPerformanceCounter(out startTime);
        }
        //结束计时
        public void Stop()
        {
            QueryPerformanceCounter(out stopTime);
        }
        //返回计时结果(ms)
        public double Duration
        {
            get
            {
                return (double)(stopTime - startTime) * 1000 / (double)freq;
            }
        }

    }
}

~~~

在Form1类内定义HiPerfTimer类并在构造函数内为其实例化
~~~csharp
        private HiPerfTimer myTimer; public Form1()
        {
            InitializeComponent();
            myTimer = new gray.HiPerfTimer();
        }

~~~

分别在“提取像素法”、“内存法”和“指针法” Button控件的 Click事件程序代码内的

if判断语句之间的最开始一行添加以下代码:
~~~csharp
            //启动计时器
            myTimer.Start();
~~~
在上述3个单击事件内的 Invalidate0语句之前添加以下代码:
~~~csharp
             //关闭计时器
 myTimer.Stop(); //在TextBox内显示计时时间
             timeBox.Text = myTimer.Duration.ToString("####.##") \+ "毫秒";
~~~
最后,编译并运行该段程序。可以明显看出,内存法和指针法比提取像素法要快得多。提取像素法应用GDI+中的方法,易于理解,方法简单,很适合于C#的初学者使用,但它的运行速度最慢,效率最低。内存法把图像复制到内存中,直接对内存中的数据进行处理,速度明显提高,程序难度也不大。指针法直接应用指针来对图像进行处理,所以速度最快。但在C#中是不建议使用指针的,因为使用指针,代码不仅难以编写和调试,而且无法通过CLR的内存类型安全检查,不能发挥C#的特长。只有对C#和指针有了充分的理解,才能用好该方法。究竟要使用哪种方法,还要看具体情况而定。但3种方法都能有效地对图像进行处理。

全文代码：
~~~csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace gray
{
    public partial class Form1 : Form
    {
        private HiPerfTimer myTimer;
        public Form1()
        {
            InitializeComponent();
            myTimer = new gray.HiPerfTimer();
        }

        //文件名
        private string curFileName;
        //图像对象
        private System.Drawing.Bitmap curBitmpap;

        /// <summary>
        /// 打开图像文件
        /// </summary>
        private void open_Click(object sender, EventArgs e)
        {
            //创建OpenFileDialog
            OpenFileDialog opnDlg = new OpenFileDialog();
            //为图像选择一个筛选器
            opnDlg.Filter = "所有图像文件|*.bmp;*.pcx;*.png;*.jpg;*.gif;" +
                "*.tif;*.ico;*.dxf;*.cgm;*.cdr;*.wmf;*.eps;*.emf|" +
                "位图(*.bmp;*.jpg;*.png;...)|*.bmp;*.pcx;*.png;*.jpg;*.gif;*.tif;*.ico|" +
                "矢量图(*.wmf;*.eps;*.emf;...)|*.dxf;*.cgm;*.cdr;*.wmf;*.eps;*.emf";
            //设置对话框标题
            opnDlg.Title = "打开图像文件";
            //启用“帮助”按钮
            opnDlg.ShowHelp = true;

            //如果结果为“打开”，选定文件
            if(opnDlg.ShowDialog()==DialogResult.OK)
            {
                //读取当前选中的文件名
                curFileName = opnDlg.FileName;
                //使用Image.FromFile创建图像对象
                try
                {
                    curBitmpap = (Bitmap)Image.FromFile(curFileName);
                }
                catch(Exception exp)
                {
                    MessageBox.Show(exp.Message);
                }
            }
            //对窗体进行重新绘制，这将强制执行paint事件处理程序
            Invalidate();
        }

        private void Form1_Paint(object sender, PaintEventArgs e)
        {
            //获取Graphics对象
            Graphics g = e.Graphics;
            if (curBitmpap != null)
            {
                //使用DrawImage方法绘制图像
                //160,20：显示在主窗体内，图像左上角的坐标
                //curBitmpap.Width, curBitmpap.Height图像的宽度和高度
                g.DrawImage(curBitmpap, 160, 20, curBitmpap.Width, curBitmpap.Height);
            }
        }

        /// <summary>
        /// 保存图像文件
        /// </summary>
        private void save_Click(object sender, EventArgs e)
        {
            //如果没有创建图像，则退出
            if (curBitmpap == null)
                return;

            //调用SaveFileDialog
            SaveFileDialog saveDlg = new SaveFileDialog();
            //设置对话框标题
            saveDlg.Title = "保存为";
            //改写已存在文件时提示用户
                saveDlg.OverwritePrompt = true;
            //为图像选择一个筛选器
                saveDlg.Filter = "BMP文件(*.bmp)|*.bmp|" + "Gif文件(*.gif)|*.gif|" + "JPEG文件(*.jpg)|*.jpg|" + "PNG文件(*.png)|*.png";
            //启用“帮助”按钮
                saveDlg.ShowHelp = true;

            //如果选择了格式，则保存图像
            if (saveDlg.ShowDialog() == DialogResult.OK)
            {
                //获取用户选择的文件名
                string filename = saveDlg.FileName;
                string strFilExtn = filename.Remove(0, filename.Length - 3);

                //保存文件
                switch (strFilExtn)
                {
                    //以指定格式保存
                    case "bmp":
                        curBitmpap.Save(filename, System.Drawing.Imaging.ImageFormat.Bmp);
                        break;
                    case "jpg":
                        curBitmpap.Save(filename, System.Drawing.Imaging.ImageFormat.Jpeg);
                        break;
                    case "gif":
                        curBitmpap.Save(filename, System.Drawing.Imaging.ImageFormat.Gif);
                        break;
                        case "tif":
                        curBitmpap.Save(filename, System.Drawing.Imaging.ImageFormat.Tiff);
                        break;
                    case "png":
                        curBitmpap.Save(filename, System.Drawing.Imaging.ImageFormat.Png);
                        break;
                    default:
                        break;
                }
            }
        }

        //关闭窗体 
        private void close_Click(object sender, EventArgs e)
        {
            this.Close();
        }

        /// <summary>
        /// 提取像素法
        /// </summary>
        private void pixel_Click(object sender, EventArgs e)
        {
            //启动计时器
            myTimer.Start();
            if (curBitmpap != null)
            {
                Color curColor;
                int ret;

                //二维图像数组循环
                for(int i = 0; i < curBitmpap.Width; i++)
                {
                    for(int j = 0; j < curBitmpap.Height; j++)
                    {
                        //获取该像素点的RGB颜色值
                        curColor = curBitmpap.GetPixel(i, j);
                        //利用公式计算灰度值
                        ret = (int)(curColor.R * 0.299 + curColor.G * 0.587 + curColor.B * 0.114);
                        //设置该像素点的灰度值，R=G=B=ret
                        curBitmpap.SetPixel(i, j, Color.FromArgb(ret, ret, ret));
                    }
                }
                //关闭计时器
                myTimer.Stop();
                //在TextBox内显示计时时间
                timeBox.Text = myTimer.Duration.ToString("####.##") + "毫秒";
                //对窗体进行重新绘制，这将强制执行Paint事件处理程序
                Invalidate();
            }
        }

        /// <summary>
        /// 内存法(适用于任意大小的24位彩色图像)
        /// </summary>
        private void memory_Click(object sender, EventArgs e)
        {
            //启动计时器
            myTimer.Start();
            if (curBitmpap != null)
            {
                //位图矩形
                Rectangle rect = new Rectangle(0, 0, curBitmpap.Width, curBitmpap.Height);
                //以可读写的方式锁定全部位图像素
                System.Drawing.Imaging.BitmapData bmpData = curBitmpap.LockBits(rect, System.Drawing.Imaging.ImageLockMode.ReadWrite, curBitmpap.PixelFormat);
                //得到首地址
                IntPtr ptr = bmpData.Scan0;

                //定义被锁定的数组大小，由位图数据与未用空间组成的
                int bytes = bmpData.Stride * bmpData.Height;
                //定义位图数组
                byte[] rgbValues = new byte[bytes];
                //复制被锁定的位图像素值到该数组内
                System.Runtime.InteropServices.Marshal.Copy(ptr, rgbValues, 0, bytes);

                //灰度化
                double colorTemp = 0;
                for (int i = 0; i < bmpData.Height; i++)
                {
                    //只处理每行中是图像像素的数据，舍弃未用空间
                    for (int j = 0; j < bmpData.Width * 3; j += 3)
                    {
                        //利用公式计算灰度值
                        colorTemp = rgbValues[i * bmpData.Stride + j + 2] * 0.299 + rgbValues[i * bmpData.Stride + j + 1] * 0.587 + rgbValues[i * bmpData.Stride + j] * 0.114;
                        //R=G=B
                        rgbValues[i * bmpData.Stride + j] = rgbValues[i * bmpData.Stride + j + 1] = rgbValues[i * bmpData.Stride + j + 2] = (byte)colorTemp;
                    }
                }

                //把数组复制回位图
                System.Runtime.InteropServices.Marshal.Copy(rgbValues, 0, ptr, bytes);
                //解锁位图像素
                curBitmpap.UnlockBits(bmpData);
                //关闭计时器
                myTimer.Stop();
                //在TextBox内显示计时时间
                timeBox.Text = myTimer.Duration.ToString("####.##") + "毫秒";
                //对窗体进行重新绘制，这将强制执行Paint事件处理程序
                Invalidate();
            }
        }

        /// <summary>
        /// 指针法
        /// </summary>
        private void pointer_Click(object sender, EventArgs e)
        {
            //启动计时器
            myTimer.Start();
            if (curBitmpap != null)
            {
                //位图矩形
                Rectangle rect = new Rectangle(0, 0, curBitmpap.Width, curBitmpap.Height);
                //以可读写的方式锁定全部位图像素
                System.Drawing.Imaging.BitmapData bmpData = curBitmpap.LockBits(rect, System.Drawing.Imaging.ImageLockMode.ReadWrite, curBitmpap.PixelFormat);

                byte temp = 0;
                //启用不安全模式
                unsafe
                {
                    //得到首地址
                    byte* ptr = (byte*)(bmpData.Scan0);
                    //二维图像循环
                    for (int i = 0; i < bmpData.Height; i++)
                    {
                        for (int j = 0; j < bmpData.Width; j++)
                        {
                            //利用公式计算灰度值
                            temp = (byte)(0.299 * ptr[2] + 0.587 * ptr[1] + 0.114 * ptr[0]);
                            //R=G=B
                            ptr[0] = ptr[1] = ptr[2] = temp;
                            //指向下一个像素
                            ptr += 3;
                        }
                        //指向下一行数组的首个字节
                        ptr += bmpData.Stride - bmpData.Width * 3;
                    }
                }
                //解锁位图像素
                curBitmpap.UnlockBits(bmpData);
                //关闭计时器
                myTimer.Stop();
                //在TextBox内显示计时时间
                timeBox.Text = myTimer.Duration.ToString("####.##") + "毫秒";
                //对窗体进行重新绘制，这将强制执行Paint事件处理程序
                Invalidate();
            }
        }

        /// <summary>
        /// 内存法(仅适用于512*512的图像)
        /// </summary>
        //private void memory_Click(object sender, EventArgs e)
        //{
        //    if (curBitmpap != null)
        //    {
        //        //位图矩形
        //        Rectangle rect = new Rectangle(0, 0, curBitmpap.Width, curBitmpap.Height);
        //        //以可读写的方式锁定全部位图像素
        //        System.Drawing.Imaging.BitmapData bmpData = curBitmpap.LockBits(rect, System.Drawing.Imaging.ImageLockMode.ReadWrite, curBitmpap.PixelFormat);
        //        //得到首地址
        //        IntPtr ptr = bmpData.Scan0;

        //        //24位bmp位图字节数
        //        int bytes = curBitmpap.Width * curBitmpap.Height * 3;
        //        //定义位图数组
        //        byte[] rgbValues = new byte[bytes];
        //        //复制被锁定的位图像素值到该数组内
        //        System.Runtime.InteropServices.Marshal.Copy(ptr, rgbValues, 0, bytes);

        //        //灰度化
        //        double colorTemp = 0;
        //        for(int i = 0; i < rgbValues.Length; i += 3)
        //        {
        //            //利用公式计算灰度值
        //            colorTemp = rgbValues[i + 2] * 0.299 + rgbValues[i + 1] * 0.587 + rgbValues[i] * 0.114;
        //            //R=G=B
        //            rgbValues[i]=rgbValues[i+1]=rgbValues[i+2]=(byte)colorTemp;
        //        }

        //        //把数组复制回位图
        //        System.Runtime.InteropServices.Marshal.Copy(rgbValues, 0, ptr, bytes);
        //        //解锁位图像素
        //        curBitmpap.UnlockBits(bmpData);
        //        //对窗体进行重新绘制，这将强制执行Paint事件处理程序
        //        Invalidate();
        //    }
        //}

    }
}
~~~
~~~csharp
/// <summary>
      /// 图像灰度化 /// </summary>
      /// <param name="bmp"></param>
      /// <returns></returns>
      public static Bitmap ToGray(Bitmap bmp)
      { for (int i = 0; i < bmp.Width; i++)
          { for (int j = 0; j < bmp.Height; j++)
              { //获取该点的像素的RGB的颜色
                  Color color = bmp.GetPixel(i, j); //利用公式计算灰度值
                  int gray = (int)(color.R * 0.3 \+ color.G * 0.59 \+ color.B * 0.11);
                  Color newColor = Color.FromArgb(gray, gray, gray);
                  bmp.SetPixel(i, j, newColor);
              }
          } return bmp;
      }
~~~

~~~ csharp
using System; using System.Collections.Generic; using System.Linq; using System.Text; using System.Threading.Tasks; using System.Runtime.InteropServices; using System.ComponentModel; using System.Threading; namespace gray
{ internal class HiPerfTimer
    { //引用win32API中的QueryPerformanceCounter()方法 //该方法用来查询任意时刻高精度计数器的实际值
        \[DllImport("Kernel32.dll")\]         //using System.Runtime.InteropServices;
        private static extern bool QueryPerformanceCounter(out long lpPerformanceCount); //引用win32API中的QueryPerformanceCounter()方法 //该方法用来查询任意时刻高精度计数器的实际值
        \[DllImport("Kernel32.dll")\] private static extern bool QueryPerformanceFrequency(out long lpFrequency); private long startTime, stopTime; private long freq; public HiPerfTimer()
        {
            startTime = 0;
            stopTime = 0; if(QueryPerformanceFrequency(out freq) == false)
            { //不支持高性能计时器
                throw new Win32Exception();     //using System.ComponentModel;
 }
        } //开始计时
        public void Start()
        { //让等待线程工作
            Thread.Sleep(0);                //using System.Threading;
 QueryPerformanceCounter(out startTime);
        } //结束计时
        public void Stop()
        {
            QueryPerformanceCounter(out stopTime);
        } //返回计时结果(ms)
        public double Duration
        { get { return (double)(stopTime - startTime) * 1000 / (double)freq;
            }
        }

    }
}
~~~

**灰度图像二值化**：  
在进行了灰度化处理之后，图像中的每个象素只有一个值，那就是象素的灰度值。它的大小决定了象素的亮暗程度。为了更加便利的开展下面的图像处理操作，还需要对已经得到的灰度图像做一个二值化处理。图像的二值化就是把图像中的象素根据一定的标准分化成两种颜色。在系统中是根据象素的灰度值处理成黑白两种颜色。和灰度化相似的，图像的二值化也有很多成熟的算法。它可以采用自适应阀值法，也可以采用给定阀值法。
~~~CSharp
#region Otsu阈值法二值化模块   

        /// <summary>   
        /// Otsu阈值 /// </summary>   
        /// <param name="b">位图流</param>   
        /// <returns></returns>   
        public Bitmap OtsuThreshold(Bitmap b)
        { // 图像灰度化 // b = Gray(b); 
            int width = b.Width; int height = b.Height; byte threshold = 0; int\[\] hist = new int\[256\]; int AllPixelNumber = 0, PixelNumberSmall = 0, PixelNumberBig = 0; double MaxValue, AllSum = 0, SumSmall = 0, SumBig, ProbabilitySmall, ProbabilityBig, Probability;
            BitmapData data = b.LockBits(new Rectangle(0, 0, width, height), ImageLockMode.ReadWrite, PixelFormat.Format32bppArgb); unsafe { byte\* p = (byte*)data.Scan0; int offset = data.Stride - width * 4; for (int j = 0; j < height; j++)
                { for (int i = 0; i < width; i++)
                    {
                        hist\[p\[0\]\]++;
                        p += 4;
                    }
                    p += offset;
                }
                b.UnlockBits(data);
            } //计算灰度为I的像素出现的概率 
            for (int i = 0; i < 256; i++)
            {
                AllSum += i * hist\[i\];     // 质量矩 
                AllPixelNumber += hist\[i\];  // 质量 
 }
            MaxValue = -1.0; for (int i = 0; i < 256; i++)
            {
                PixelNumberSmall += hist\[i\];
                PixelNumberBig = AllPixelNumber - PixelNumberSmall; if (PixelNumberBig == 0)
                { break;
                }

                SumSmall += i * hist\[i\];
                SumBig = AllSum - SumSmall;
                ProbabilitySmall = SumSmall / PixelNumberSmall;
                ProbabilityBig = SumBig / PixelNumberBig;
                Probability = PixelNumberSmall * ProbabilitySmall * ProbabilitySmall + PixelNumberBig * ProbabilityBig * ProbabilityBig; if (Probability > MaxValue)
                {
                    MaxValue = Probability;
                    threshold = (byte)i;
                }
            } return this.Threshoding(b, threshold);
        } // end of OtsuThreshold 2 
        #endregion

        #region      固定阈值法二值化模块
        public Bitmap Threshoding(Bitmap b, byte threshold)
        { int width = b.Width; int height = b.Height;
            BitmapData data = b.LockBits(new Rectangle(0, 0, width, height), ImageLockMode.ReadWrite, PixelFormat.Format32bppArgb); unsafe { byte\* p = (byte*)data.Scan0; int offset = data.Stride - width * 4; byte R, G, B, gray; for (int y = 0; y < height; y++)
                { for (int x = 0; x < width; x++)
                    {
                        R = p\[2\];
                        G = p\[1\];
                        B = p\[0\];
                        gray = (byte)((R * 19595 \+ G * 38469 \+ B * 7472) >\> 16); if (gray >= threshold)
                        {
                            p\[0\] = p\[1\] = p\[2\] = 255;
                        } else {
                            p\[0\] = p\[1\] = p\[2\] = 0;
                        }
                        p += 4;
                    }
                    p += offset;
                }
                b.UnlockBits(data); return b;

            }

        } #endregion
~~~