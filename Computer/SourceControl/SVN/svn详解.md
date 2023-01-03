​

转自：[svn status详解 - 世界，太精彩 - 博客园](https://www.cnblogs.com/cina33blogs/p/6805905.html "svn status详解 - 世界，太精彩 - 博客园")

    
svn status打印<font color="red">五列字符</font>，紧跟一些空格，接着是文件或者目录名。  
**<font color="red">第一列</font>告诉一个文件的状态或它的内容，返回代码解释如下：**  
**?** item 文件、目录或是符号链item不在版本控制之下，你可以通过使用svn status的--quiet（-q）参数或父目录的svn:ignore属性忽略这个问题，关于忽略文件的使用，见“svn:ignore”一节。  

**!** item 文件、目录或是符号链item在版本控制之下，但是已经丢失或者不完整，这可能因为使用非Subversion命令删除造成的，如果是一个目录，有可能是检出或是更新时的中断造成的，使用svn update可以重新从版本库获得文件或者目录，也可以使用svn revert file恢复原来的文件。  

**~**  item 文件、目录或是符号链item在版本库已经存在，但你的工作拷贝中的是另一个。举一个例子，你删除了一个版本库的文件，新建了一个在原来的位置，而且整个过程中没有使用svn delete或是svn add

**A** item 文件、目录或是符号链item预定加入到版本库。  

**C** item 文件item发生冲突，在从服务器更新时与本地版本发生交迭，在你提交到版本库前，必须手工的解决冲突。  

**D** item 文件、目录或是符号链item预定从版本库中删除。  

**M** item 文件item的内容被修改了。  

**R** item 文件、目录或是符号链item预定将要替换版本库中的item，这意味着这个对象首先要被删除，另外一个同名的对象将要被添加，所有的操作发生在一个修订版本。  

**X** item 目录没有版本化，但是与Subversion的外部定义关联，关于外部定义，可以看“外部定义”一节。  
原来的位置，而且整个过程中没有使用svn delete或是svn add。  

**I** item 文件、目录或是符号链item不在版本控制下，Subversion已经配置好了会在svn add、svn import和svn status命令忽略这个文件，关于忽略文件，见“svn:ignore”一节。注意，这个符号只会在使用svn status的参数--no-ignore时才会出现—否则这个文件会被忽略且不会显示！  
> ?：不在svn的控制中
A：**A**dd，新增
C：**C**onflict，冲突; tc以他们改得为准
D：**D**elete，删除
M：**M**odify，本地已经修改
G：modify and mer**G**ed，本地文件修改并且和服务器的进行合并
U：**U**pdate，从服务器更新
R：**R**eplace，从服务器替换
I：**I**gnored，忽略
K：loc**K**，文件被锁定

**<font color="red">第二列</font>只显示空白或者M，说明文件或目录的属性的状态**（更多细节可以看“属性”一节），  
如果一个M出现在第二列，说明属性被修改了，否则显示空白。

**<font color="red">第三列</font>只显示空白或者L**，  
L表示Subversion已经在.svn工作区域锁定了这个项目，当你的svn commit正在运行的时候—也许正在输入log信息，运行svn status你可以看到L标记，如果这时候Subversion并没有运行，可以推测Subversion发生中断并且已经锁定，你必须运行svn cleanup来清除锁定（本节后面将有更多论述）。

  
**<font color="red">第四列</font>只会显示空白或+**，  
+的意思是一个有附加历史信息的文件或目录预定添加或者修改到版本库，通常出现在svn move或是svn copy时，如果是看到A  +就是说要包含历史的增加，它可以是一个文件或是拷贝的根目录。+表示它是即将包含历史增加到版本库的目录的一部分，也就是说他的父目录要拷贝，它只是跟着一起的。 M  +表示将要包含历史的增加，并且已经更改了。当你提交时，首先会随父目录进行包含历史的增加，然后本地的修改提交到更改后的版本。

**<font color="red">第五列</font>只显示空白或是S**，  
表示这个目录或文件已经转到了一个分支下了（使用svn switch）。

​
**svn st 命令用来查看本地文本和版本库里面的文件的区别（在提交前）。返回值有许多种具体含义如下：**

L      abc.c               # svn已经在.svn目录锁定了abc.c  
M      bar.c                  # bar.c的内容已经在本地修改过了  
M      baz.c                 # baz.c属性有修改，但没有内容修改  
X      3rd_party           # 这个目录是外部定义的一部分  
?      foo.o               # svn并没有管理foo.o  
!      some_dir            # svn管理这个，但它可能丢失或者不完整  
~      qux                 # 作为file/dir/link进行了版本控制，但类型已经改变  
I      .screenrc           # svn不管理这个，配置确定要忽略它  
A  +   moved_dir           # 包含历史的添加，历史记录了它的来历  
M  +   moved_dir/README    # 包含历史的添加，并有了本地修改  
D      stuff/fish.c        # 这个文件预定要删除  
A      stuff/loot/bloo.h   # 这个文件预定要添加  
C      stuff/loot/lump.c   # 这个文件在更新时发生冲突  
R      xyz.c               # 这个文件预定要被替换  
S      stuff/squawk        # 这个文件已经跳转到了分支  