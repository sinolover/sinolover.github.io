## 一、QString转number
~~~cpp
QString number(long n, int base = 10)

QString number(ulong n, int base = 10)

QString number(int n, int base = 10)

QString number(uint n, int base = 10)

QString number(qlonglong n, int base = 10)

QString number(qulonglong n, int base = 10)

QString number(double n, char format = 'g', int precision = 6)
~~~


整形的转换格式都是一样的，第一个参数是十进制要转换的整数，第二个参数指定以什么进制来转换，默认是十进制，比如：
~~~cpp
QString strNumDec = QString::number(55, 10);  //转化成10进制
QString strNumHex = QString::number(55, 16);    //16进制
QString strNumBit = QString::number(55, 2);     //2进制
~~~
第二个参数base必须在\[2,36\]之间，当base为10以外的值时，第一个参数n将被视为无符号整数。

## 二、number 转 QString
~~~cpp
double toDouble(bool \* ok = 0) const
float toFloat(bool \* ok = 0) const
int toInt(bool \* ok = 0, int base = 10) const
long toLong(bool \* ok = 0, int base = 10) const qlonglong toLongLong(bool \* ok = 0, int base = 10) const
short toShort(bool \* ok = 0, int base = 10) const
~~~

QString也提供了一系列转换成数值的函数，参数ok指示转换是否出错，参数base指示当前QString是什么进制，如
~~~cpp

QString str = "55"; bool ok; int numBit = str.toInt(&ok, 2); int numOct = str.toInt(&ok, 8); int numDec = str.toInt(&ok, 10); int numHex = str.toInt(&ok, 16);
~~~

##  三、QPixmap加载图片并获取图片宽和高
~~~cpp
void Dialog::paintEvent(QPaintEvent *)
{
    QPainter painter(this);
    QPixmap pix;
    //加载图片
    pix.load("D:\\1001.jpg");
    //获得图片的宽和高
    qreal width = pix.width(); 
    qreal height = pix.height();
    //图片放大两倍
    pix = pix.scaled(width*2,height*2,Qt::KeepAspectRatio);
    
    painter.drawPixmap(0,0,100,100,pix);
}
~~~

## 四、QString与String转换
~~~cpp
//QString转换String
string s = qstr.toStdString();

//String转换QString

QString qstr2 = QString::fromStdString(s);
~~~
这样虽然能成功，但是会出现中文乱码情况。

转化与乱码处理

~~~cpp
std::string cstr;
QString qstring;

//从std::string 到QString
qstring = QString(QString::fromLocal8Bit(cstr.c_str()));

//从QString 到 std::string
cstr = string((const char *)qstring.toLocal8Bit());

//不需要从gbk转到utf8
QString value_content = QString::fromStdString(vec[i].content);
QString value_classname = QString::fromStdString(vec[i].classname);

~~~

##  五、判断文件或者文件夹是否存在

1. 判断文件夹是不是存在  
参数说明：  
QString fullPath;//文件夹全路径
~~~cpp
/*方法1*/
bool isDirExist(QString fullPath)
{
    QDir dir(fullPath);
    if(dir.exists())
    {
      return true;
    }
    return false;
}
/*方法2*/
bool isDirExist(QString fullPath)
{
    QFileInfo fileInfo(fullPath);
    if(fileInfo.isDir())
    {
      return true;
    }
    return false;
}
~~~
 

2. 判断文件是不是存在

~~~cpp
参数说明：
QString fullFileName;//文件全路径(包含文件名)

bool isFileExist(QString fullFileName)
{
    QFileInfo fileInfo(fileFullName);
    if(fileInfo.isFile())
    {
        return true;
    }
    return false;
}
~~~

## 六、图像旋转

### 第一种方案

使用 QPixmap 的 transformed 函数来实现旋转，这个函数默认是以图片中心为旋转点，不能设置旋转的中心点，使用如下：

~~~cpp
QMatrix matrix;
matrix.rotate(45);

QLabel *Label= new QLabel();
Label->setPixmap(QPixmap(“:/images.png”).transformed(matrix, Qt::SmoothTransformation));
~~~

该段程序实现的效果是使图片顺时针旋转 45 度。

### 第二种方案

使用 QPainter 这位“画家”，示例程序如下：

~~~cpp

void Widget::paintEvent(QPaintEvent *)
{
    QPainter painter(this);
    QPixmap disc(":/disc.png");

    /* 碟机转动 */
    if(imageRotate++ == 360)
        imageRotate = 0;
    /* 设定旋转中心点 */
    painter.translate(130,150);
    /* 旋转的角度 */
    painter.rotate(imageRotate);
    /* 恢复中心点 */
    painter.translate(-130,-150);
    /* 画图操作 */
    painter.drawPixmap(40,60,180,180, disc);
}
~~~