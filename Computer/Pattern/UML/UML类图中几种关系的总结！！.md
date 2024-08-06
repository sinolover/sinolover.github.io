# UML类图中几种关系的总结！！
**九种关系总结，EA图中会用到：  **

**关联关系**(Association)：双向关联，单向关联，自关联、多重性关联Multiplicity、

**聚合**(Aggregation)：整体与部分的关系，整体对象销毁时成员对象不销毁，一般是构造函数或Set方法传入成员对象。

**组合**(Composition)：整体与部分的关系，整体对象销毁时成员对象一并销毁，一般在构造函数中创建成员对象。

**依赖关系****(Dependency)：Driver类依赖Car类的move方法，****Driver****\--->****Car******

**泛化关系**(Generalization)：父类与子类之间，由子类指向父类

**实现关系**(Realization)：接口与实现类之间，由实现类指向接口*

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在UML类图中，常见的有以下几种关系: 泛化（Generalization）,  实现（Realization），关联（Association)，聚合（Aggregation），组合(Composition)，依赖(Dependency)  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1. 泛化（Generalization）**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【泛化关系】：是一种继承关系，表示一般与特殊的关系，它指定了子类如何特化父类的所有特征和行为。例如：老虎是动物的一种，即有老虎的特性也有动物的共性。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【箭头指向】：带三角箭头的实线，箭头指向父类

![UML类图几种关系的总结](vx_images/211844409267688.png) 

 **2. 实现（Realization）**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【实现关系】：是一种类与接口的关系，表示类是接口所有特征和行为的实现.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【箭头指向】：带三角箭头的虚线，箭头指向接口

![UML类图几种关系的总结](vx_images/209794409270084.png) 

 **3. 关联（Association)**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【关联关系】：是一种拥有的关系，它使一个类知道另一个类的属性和方法；如：老师与学生，丈夫与妻子关联可以是双向的，也可以是单向的。双向的关联可以有两个箭头或者没有箭头，单向的关联有一个箭头。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【代码体现】：成员变量

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【箭头及指向】：带普通箭头的实心线，指向被拥有者

![UML类图几种关系的总结](vx_images/207714409252204.png) 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;上图中，老师与学生是双向关联，老师有多名学生，学生也可能有多名老师。但学生与某课程间的关系为单向关联，一名学生可能要上多门课程，课程是个抽象的东西他不拥有学生。 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;下图为自身关联： 

![UML类图几种关系的总结](vx_images/205644409256450.png)

 **4. 聚合（Aggregation）**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【聚合关系】：是整体与部分的关系，且部分可以离开整体而单独存在。如车和轮胎是整体和部分的关系，轮胎离开车仍然可以存在。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;聚合关系是关联关系的一种，是强的关联关系；关联和聚合在语法上无法区分，必须考察具体的逻辑关系。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【代码体现】：成员变量

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【箭头及指向】：带空心菱形的实心线，菱形指向整体

![UML类图几种关系的总结](vx_images/203374409259895.png) 

 **5. 组合(Composition)**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【组合关系】：是整体与部分的关系，但部分不能离开整体而单独存在。如公司和部门是整体和部分的关系，没有公司就不存在部门。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;组合关系是关联关系的一种，是比聚合关系还要强的关系，它要求普通的聚合关系中代表整体的对象负责代表部分的对象的生命周期。

【代码体现】：成员变量

【箭头及指向】：带实心菱形的实线，菱形指向整体

![UML类图几种关系的总结](vx_images/201294409267226.png)

 **6. 依赖(Dependency)**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【依赖关系】：是一种使用的关系，即一个类的实现需要另一个类的协助，所以要尽量不使用双向的互相依赖.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【代码表现】：局部变量、方法的参数或者对静态方法的调用

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【箭头及指向】：带箭头的虚线，指向被使用者

![UML类图几种关系的总结](vx_images/198924409247060.png) 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;各种关系的强弱顺序：

 **泛化 = 实现 > 组合 > 聚合 > 关联 > 依赖** 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;下面这张UML图，比较形象地展示了各种类图关系：

![UML类图几种关系的总结](vx_images/196844409259193.png)

转自：http://blog.csdn.net/tianhai110/article/details/6339565