# C# 获取文件名及扩展名
~~~csharp
C#通过文件路径获取文件名
string fullPath = @"/WebSite1/Default.aspx";

string filename = System.IO.Path.GetFileName(fullPath);//文件名 “Default.aspx”
string extension = System.IO.Path.GetExtension(fullPath);//扩展名 “.aspx”
string fileNameWithoutExtension = System.IO.Path.GetFileNameWithoutExtension(fullPath);// 没有扩展名的文件名 “Default”


string aFirstName = aFile.Substring(aFile.LastIndexOf("\\") + 1, (aFile.LastIndexOf(".") - aFile.LastIndexOf("\\") - 1)); //文件名
string aLastName = aFile.Substring(aFile.LastIndexOf(".") + 1, (aFile.Length - aFile.LastIndexOf(".") - 1)); //扩展名

string strFilePaht="文件路径";
Path.GetFileNameWithoutExtension(strFilePath);这个就是获取文件名的

还有的就是用Substring截取
strFilePaht.Substring(path.LastIndexOf("\\") + 1, path.Length - 1 - path.LastIndexOf("\\"));
strFilePaht.Substring(path.LastIndexOf("."), path.Length - path.LastIndexOf("."));

或者用openFileDialog1.SafeFileName

这样就能取到该文件的所在目录路径
string path1 = System.IO.Path.GetDirectoryName(openFileDialog1.FileName) + @"\";

string path = Path.GetFileName("C:\My Document\path\image.jpg"); //只获取文件名image.jpg


/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////


string fullPath = @"\WebSite1\Default.aspx";

string filename = System.IO.Path.GetFileName(fullPath);//文件名 “Default.aspx”
string extension = System.IO.Path.GetExtension(fullPath);//扩展名 “.aspx”
string fileNameWithoutExtension = System.IO.Path.GetFileNameWithoutExtension(fullPath);// 没有扩展名的文件名 “Default”

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

System.IO.Path.GetFileNam(filePath) //返回带扩展名的文件名 
System.IO.Path.GetFileNameWithoutExtension(filePath) //返回不带扩展名的文件名 
System.IO.Path.GetDirectoryName(filePath) //返回文件所在目录

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

//获取当前进程的完整路径，包含文件名(进程名)。
string str = this.GetType().Assembly.Location;
result: X:\xxx\xxx\xxx.exe (.exe文件所在的目录+.exe文件名)

//获取新的 Process 组件并将其与当前活动的进程关联的主模块的完整路径，包含文件名(进程名)。
string str = System.Diagnostics.Process.GetCurrentProcess().MainModule.FileName;
result: X:\xxx\xxx\xxx.exe (.exe文件所在的目录+.exe文件名)

//获取和设置当前目录（即该进程从中启动的目录）的完全限定路径。
string str = System.Environment.CurrentDirectory;
result: X:\xxx\xxx (.exe文件所在的目录)

//获取当前 Thread 的当前应用程序域的基目录，它由程序集冲突解决程序用来探测程序集。
string str = System.AppDomain.CurrentDomain.BaseDirectory;
result: X:\xxx\xxx\ (.exe文件所在的目录+"\")

//获取和设置包含该应用程序的目录的名称。
string str = System.AppDomain.CurrentDomain.SetupInformation.ApplicationBase;
result: X:\xxx\xxx\ (.exe文件所在的目录+"\")

//获取启动了应用程序的可执行文件的路径，不包括可执行文件的名称。
string str = System.Windows.Forms.Application.StartupPath;
result: X:\xxx\xxx (.exe文件所在的目录)

//获取启动了应用程序的可执行文件的路径，包括可执行文件的名称。
string str = System.Windows.Forms.Application.ExecutablePath;
result: X:\xxx\xxx\xxx.exe (.exe文件所在的目录+.exe文件名)

//获取应用程序的当前工作目录(不可靠)。
string str = System.IO.Directory.GetCurrentDirectory();
result: X:\xxx\xxx (.exe文件所在的目录)

//直接打开文件 
System.Diagnostics.Process.Start("C:\\My Document\\path\\image.jpg"); //打开此文件。

//直接打开文件位置
System.Diagnostics.ProcessStartInfo psi = new System.Diagnostics.ProcessStartInfo("Explorer.exe");
psi.Arguments = "/e,/select," + "C:\\My Document\\path\\image.jpg";
System.Diagnostics.Process.Start(psi);
转自http://libushuang.cnblogs.com
~~~