 

DAX: Data Analysis eXpression：类似Excel函数

vlookup

需要查找的 关键字

查找的范围

找到数据后，返回查找范围中的第几列的数据

01.从数据库导出数据到平面文件

平面文件：

02.平面文件输出格式的区别

平面文件的不同输出格式

delimited

Fixed width

Fixed Width with Row Delimiters最后一列依然是定宽，然后跟分隔符（直观上，分隔符单独成一列）

Ragged Right：直接换行（分隔符跟在行末）

03.从平面文件导入数据到数据库一

将平面文件中的“引号分隔符去掉，

控制流

DFT：data flow task

EST：Execute SQL Task

数据流

FF  ：Flat File Sources

OLE DB Destination

CM：Connection Manager

STAGING表一般一次性加载，即：在加载前，将原表清空

04.从平面文件导入数据到数据库二

fixed width 文件需要将每一个字段的宽度量出来，填好

05.平面文件的空值处理

Fast Load 概念：选中Table Lock Check Constraints。他对空值Null 处理取决于KeepNull

常见选项含义（fastload)

Keep Identity：是指来自于上游的数据，含有Identity标识符，将替换目标表的相应字段（选中后，可以保留源表的ID列值 到 目标表中）

Table Lock：插入数据时，表级锁定

Keep Nulls：如果未选则根据目标表是否有default，有的话，使用default。

Check Constraints：是否 运行表中的 检查约束（若不选择，则插入的数据可以违反目标表中相应的检查约束）：一般不选择，这样可以提高插入效率（前提是，源表不存在违反约束的数据）

Data access mode：

Table or View 一行一行插入，速度慢

Table or View -fast load 类似于SQL中bulk insert 批量插入，要么成功要么失败。一般选择该选项

![](file:///C:\Users\steven\AppData\Local\Temp\ksohtml17000\wps2.jpg) 

06.不规则的平面文件的输出技巧

07.Error Output 错误输出

理解Error 和 Truncation的区别

Error ：发生错误

Truncation：字符串发生截断

理解Fail component，Ignore Failure各自左右

redirect row 重定向

DC Data Convert 数据转换