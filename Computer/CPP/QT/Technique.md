Qt：正确判断文件、文件夹是否存在的方法
一直对Qt的isFile、isDir、exists这几个方法感到混乱，不知道到底用哪个，网上查了下资料，也是用这几个方法，但是都没有对其深究，经过测试发现会存在问题，先看看下面的测试代码

```cpp
{
    QFileInfo fi("C:/123");                     // 目录存在
    qDebug() << fi.isFile();                    // false
    qDebug() << fi.isDir();                     // true
    qDebug() << fi.exists();                    // true
    qDebug() << fi.isRoot();                    // false
    qDebug() << QFile::exists("C:/123");        // true
    qDebug() << QDir("C:/123").exists();        // true

    fi.setFile("C:/ABC");                       // 目录不存在
    qDebug() << fi.isFile();                    // false
    qDebug() << fi.isDir();                     // false
    qDebug() << fi.exists();                    // false
    qDebug() << fi.isRoot();                    // false
    qDebug() << QFile::exists("C:/ABC");        // false
    qDebug() << QDir("C:/ABC").exists();        // false

    fi.setFile("C:/");                          // 存在的驱动器
    qDebug() << fi.isFile();                    // false
    qDebug() << fi.isDir();                     // true
    qDebug() << fi.exists();                    // true
    qDebug() << fi.isRoot();                    // true
    qDebug() << QFile::exists("C:/");           // true
    qDebug() << QDir("C:/").exists();           // true

    fi.setFile("Z:/");                          // 不存在的驱动器
    qDebug() << fi.isFile();                    // false
    qDebug() << fi.isDir();                     // false
    qDebug() << fi.exists();                    // false
    qDebug() << fi.isRoot();                    // false
    qDebug() << QFile::exists("Z:/");           // false
    qDebug() << QDir("Z:/").exists();           // false

    fi.setFile("C:/123.lnk");                   // 快捷方式存在且指向的文件也存在
    qDebug() << fi.isFile();                    // true
    qDebug() << fi.isDir();                     // false
    qDebug() << fi.exists();                    // true
    qDebug() << fi.isRoot();                    // false
    qDebug() << QFile::exists("C:/123.lnk");    // true
    qDebug() << QDir("C:/123.lnk").exists();    // false

    fi.setFile("C:/456.lnk");                   // 快捷方式存在但指向的文件不存在
    qDebug() << fi.isFile();                    // false
    qDebug() << fi.isDir();                     // false
    qDebug() << fi.exists();                    // false
    qDebug() << fi.isRoot();                    // false
    qDebug() << QFile::exists("C:/456.lnk");    // false
    qDebug() << QDir("C:/456.lnk").exists();    // false
}
```

可以看到，容易让人感到混乱的是exists方法，这个方法是通用的判断方法，可以看成是这样的表达式

```
exists() == (isFile() || isDir())
```

这也是我想说明的问题，网上一些文章中用`exists`方法是不严谨的。比如你的本意是判断文件是否存在，但文件不存在，而恰巧有个同名的文件夹，那么`exists`也会返回`true`。文件夹也是同理。

根据上面的代码作出的结论：

*   **准确判断文件是否存在**

1.  用`QFileInfo::isFile()`方法

*   **准确判断文件夹是否存在**

1.  用`QFileInfo::isDir()`方法
2.  用`QDir::exists()`方法

*   **不关心是文件还是文件夹**

1.  用`QFileInfo::exists()`方法
2.  用`QFile::exists()`方法