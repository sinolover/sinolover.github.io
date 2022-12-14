三个概念

*   数据库：数据库是一个仓库，在仓库中可以存放集合
*   集合：集合类似于数组，在集合中存放文档
*   文档：最小单位，所有的操作，增删改查都是针对文档的
*   在MongoDB中，数据库、集合都不需要手动创建。当我们创建文档时，若文档所在的集合、数据库不存在，MongoDB会自动创建。也就是说，可以使用use 打开任意名称的数据库，无论这个名称的数据库是否存在。若use的是不存在的数据库，那么，当我们添加新的文档时，MongoDB会首先创建以 这个 名字 为名的数据库，然后再插入文档

show dbs / show databases 显示所有数据库

use DBNAME 选择数据库

db ：是一个变量，存放的是 当前的数据库名字

show collections 显示当前数据库中，所有的集合

CRUD create read update delete

db..insert({name:"zhangsan",age:18,gender:"maile"})

db..find(); .find({name:"zhangsan"}) :返回值是数组

*   find().length() / .count()
*   find().sort({FILEDNAME:ORDER}) 1升序 -1 降序

db..findOne()：返回值是对象

db..update(查找t条件对象，修改对象)

*   $set 修改或增加文档中，指定的属性
*   $unset 删除指定属性

db..remove

db..deleteOne

db..deleteMany

db..drop() 删除集合

db.dropDatabase() 删除数据库

文档之间的关系

*   1对1
*   1对多
*   多对多