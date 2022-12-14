一、在MySQL下查看当前使用的是哪个数据库，有三种方式
（1）用select database()语句
mysql> select database();
+------------+
| database() |
+------------+
| test       |
+------------+
1 row in set (0.00 sec)
从查询结果中可以看出，当前用的是test数据库
 
（2）用show tables语句，查询出来的结果中，第一行为Tables_in_XXX，这里XXX就
是当前所用的数据库名称。
mysql> show tables;
+-------------------+
| Tables_in_test    |
+-------------------+
| push_test         |
| ship_order_detail |
+-------------------+
2 rows in set (0.00 sec)
从查询结果中可以看出，当前用的是test数据库。
 
（3）用status语句，查询出来的结果中有一行是currrent database: XXX。这里XXX就
是当前所用的数据库名称。
mysql> status;
--------------
mysql  Ver 14.14 Distrib 5.1.60, for pc-linux-gnu (i686) using  EditLine wrapper
 
Connection id:          1484237
Current database:       test
Current user:           root@localhost
SSL:                    Not in use
从查询结果中可以看出，当前用的是test数据库。

SHOW GRANTS [FOR 'user'[@'host']]
为给定用户列出GRANT语句。如果没有指定FOR语句，默认为当前用户。
 SELECT DATABASE();
显示当前数据库。
SELECT CURRENT_USER();
显示当前mysql服务器连接的用户名和主机的组合。
SELECT USER();
显示当前会话连接的用户名和主机的组合。
SHOW CREATE TABLE
SHOW CREATE TABLE tbl_name 
Shows the CREATE TABLE statement that creates the given table.
 
SHOW CREATE {DATABASE | SCHEMA} db_name 
Shows the CREATE DATABASE statement that creates the given database. SHOW CREATE SCHEMA is a synonym forSHOW CREATE DATABASE.
二、MySQL中SELECT命令类似于其他编程语言里的print或者write，你可以用它来显示一个字符串、数字、数学表达式的结果等等。

 
示例0：显示当前数据库
格式：select database();
示例1：显示MYSQL的版本
mysql> select version();
+------------+
| version() |
+------------+
| 5.5.40     |
+------------+
1 row in set (0.01 sec)
 
示例2：显示当前时间
mysql> select now();
+----------------------------+
| now()                          |
+----------------------------+
| 2015-02-25 15:54:13 |
+----------------------------+
1 row in set (0.00 sec)
 
示例3：显示年月日
mysql> SELECT DAYOFMONTH(CURRENT_DATE);
+---------------------------------------+
| DAYOFMONTH(CURRENT_DATE) |
+---------------------------------------+
|                                             25 |
+---------------------------------------+
1 row in set (0.00 sec)
  
mysql> SELECT MONTH(CURRENT_DATE);
+-------------------------------+
| MONTH(CURRENT_DATE) |
+-------------------------------+
|                                     2 |
+-------------------------------+
1 row in set (0.00 sec)
  
mysql> SELECT YEAR(CURRENT_DATE);
+----------------------------+
| YEAR(CURRENT_DATE) |
+----------------------------+
|                            2015 |
+----------------------------+
1 row in set (0.00 sec)
 
示例4：显示字符串
mysql> SELECT "www.cmsjzy.cn";
+--------------------+
| www.cmsjzy.cn |
+--------------------+
| www.cmsjzy.cn |
+--------------------+
1 row in set (0.00 sec)
 
示例5：当计算器用
mysql> select ((6 * 8) / 10 ) + 66;
+------------------------+
| ((6 * 8) / 10 ) + 66 |
+------------------------+
|                 70.8000 |
+------------------------+
1 row in set (0.00 sec)
 
示例6：串接字符串
mysql> select CONCAT(f_name, " ", l_name) 
    -> AS Name 
    -> from employee_data 
    -> where title = 'Marketing Executive'; 
+-------------------+ 
| Name               | 
+-------------------+ 
| Monica Sehgal  | 
| Hal Simlai         | 
| Joseph Irvine   | 
+-------------------+ 
3 rows in set (0.00 sec) 
 
注意：这里用到CONCAT()函数，用来把字符串串接起来。另外，我们还用到以前学到的AS给结果列'CONCAT(f_name, " ", l_name)'起了个假名。


三、杂项
问题：
MYSQL语句如何列出当前数据库的所有表格？

-----------------------------------------------------------------------
答案1[推荐答案]：
show   tables

-----------------------------------------------------------------------
答案2[推荐答案]：
select   name 
from   sysobjects 
where   xtype= 'u '

-----------------------------------------------------------------------
答案3[推荐答案]：
show   databases; 
use   'database ' 
show   tables;

-----------------------------------------------------------------------
答案4：
二、显示命令   
1、显示数据库列表。   
show   databases;   
刚开始时才两个数据库：mysql和test。mysql库很重要它里面有MYSQL的系统信息，我们改密码和新增用户，实际上就是用这个库进行操作。   
2、显示库中的数据表：   
use   mysql；   ／／打开库，学过FOXBASE的一定不会陌生吧   
show   tables;   
3、显示数据表的结构：   
describe   表名;   
4、建库：   
create   database   库名;   
5、建表：   
use   库名；   
create   table   表名   (字段设定列表)；   
6、删库和删表:   
drop   database   库名;   
drop   table   表名；   
7、将表中记录清空：   
delete   from   表名;   
8、显示表中的记录：   
select   *   from   表名;   
三、一个建库和建表以及插入数据的实例   
drop   database   if   exists   school;   //如果存在SCHOOL则删除   
create   database   school;   //建立库SCHOOL   
use   school;   //打开库SCHOOL   
create   table   teacher   //建立表TEACHER   
(   
id   int(3)   auto_increment   not   null   primary   key,   
name   char(10)   not   null,   
address   varchar(50)   default   ’深圳’,   
year   date   
);   //建表结束   
//以下为插入字段   
insert   into   teacher   values(’’,’glchengang’,’深圳一中’,’1976-10-10’);   
insert   into   teacher   values(’’,’jack’,’深圳一中’,’1975-12-23’);   
注：在建表中
（1）将ID设为长度为3的数字字段:int(3)并让它每个记录自动加一:auto_increment并不能为空:not   null而且让他成为主字段primary   key
（2）将NAME设为长度为10的字符字段
（3）将ADDRESS设为长度50的字符字段，而且缺省值为深圳。varchar和char有什么区别呢，只有等以后的文章再说了。（4）将YEAR设为日期字段。   
如果你在mysql提示符键入上面的命令也可以，但不方便调试。你可以将以上命令原样写入一个文本文件中假设为school.sql，然后复制到c:\\下，并在DOS状态进入目录\\mysql\\bin，然后键入以下命令：   
mysql   -uroot   -p密码   <   c:\\school.sql   
如果成功，空出一行无任何显示；如有错误，会有提示。（以上命令已经调试，你只要将//的注释去掉即可使用）。   
四、将文本数据转到数据库中   
1、文本数据应符合的格式：字段数据之间用tab键隔开，null值用\\n来代替.   
例：   
3   rose   深圳二中   1976-10-10   
4   mike   深圳一中   1975-12-23   
2、数据传入命令   load   data   local   infile   \ "文件名\ "   into   table   表名;   
注意：你最好将文件复制到\\mysql\\bin目录下，并且要先用use命令打表所在的库。   
五、备份数据库：（命令在DOS的\\mysql\\bin目录下执行）   
mysqldump   --opt   school> school.bbb   
注释:将数据库school备份到school.bbb文件，school.bbb是一个文本文件，文件名任取，打开看看你会有新发现。   

-----------------------------------------------------------------------
答案5：

show   tables

-----------------------------------------------------------------------
答案6：
引用 5 楼 succeese 的回复:

1、显示数据库列表。 
show databases; 
刚开始时才两个数据库：mysql和test。mysql库很重要它里面有MYSQL的系统信息，我们改密码和新增用户，实际上就是用这个库进行操作。 
2、显示库中的数据表： 
use mysql； ／／打开库，学过FOXBASE的一定不会陌生吧 
show tab……


赞一个！

-----------------------------------------------------------------------
答案7：
引用 5 楼 succeese 的回复:

二、显示命令 
1、显示数据库列表。 
show databases; 
刚开始时才两个数据库：mysql和test。mysql库很重要它里面有MYSQL的系统信息，我们改密码和新增用户，实际上就是用这个库进行操作。 
2、显示库中的数据表： 
use mysql； ／／打开库，学过FOXBASE的一定不会陌生吧 
show tables; 
3、显示数据表的结构： 
describe 表名; 
4、建库： 
create database 库名; 
5、建表： 
use 库名； 
create table 表名 (字段设定列表)； 
6、删库和删表: 
drop database 库名; 
drop table 表名； 
7、将表中记录清空： 
delete from 表名; 
8、显示表中的记录： 
select * from 表名; 
三、一个建库和建表以及插入数据的实例 
drop database if exists school; //如果存在SCHOOL则删除 
create database school; //建立库SCHOOL 
use school; //打开库SCHOOL 
create table teacher //建立表TEACHER 
( 
id int(3) auto_increment not null primary key, 
name char(10) not null, 
address varchar(50) default ’深圳’, 
year date 
); //建表结束 
//以下为插入字段 
insert into teacher values(’’,’glchengang’,’深圳一中’,’1976-10-10’); 
insert into teacher values(’’,’jack’,’深圳一中’,’1975-12-23’); 
注：在建表中（1）将ID设为长度为3的数字字段:int(3)并让它每个记录自动加一:auto_increment并不能为空:not null而且让他成为主字段primary key（2）将NAME设为长度为10的字符字段（3）将ADDRESS设为长度50的字符字段，而且缺省值为深圳。varchar和char有什么区别呢，只有等以后的文章再说了。（4）将YEAR设为日期字段。 
如果你在mysql提示符键入上面的命令也可以，但不方便调试。你可以将以上命令原样写入一个文本文件中假设为school.sql，然后复制到c:\\下，并在DOS状态进入目录\\mysql\\bin，然后键入以下命令： 
mysql -uroot -p密码 < c:\\school.sql 
如果成功，空出一行无任何显示；如有错误，会有提示。（以上命令已经调试，你只要将//的注释去掉即可使用）。 
四、将文本数据转到数据库中 
1、文本数据应符合的格式：字段数据之间用tab键隔开，null值用\\n来代替. 
例： 
3 rose 深圳二中 1976-10-10 
4 mike 深圳一中 1975-12-23 
2、数据传入命令 load data local infile \ "文件名\ " into table 表名; 
注意：你最好将文件复制到\\mysql\\bin目录下，并且要先用use命令打表所在的库。 
五、备份数据库：（命令在DOS的\\mysql\\bin目录下执行） 
mysqldump --opt school> school.bbb 
注释:将数据库school备份到school.bbb文件，school.bbb是一个文本文件，文件名任取，打开看看你会有新发现。 