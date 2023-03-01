<h1>剖析ASP.NET AJAX的面向对象思想</h1>
人们期待已久的ASP.NET AJAX v1.0正式版终于发布了。现在你能用Microsoft ASP.NET AJAX的javascript很容易的写出丰富的、交互式的web应用。尤其值得关注的是Microsoft AJAX Library增加了面向对象的支持，而以前javascript是不支持面向对象开发的。现在icrosoft AJAX Library能很好的支持类、名字空间、继承、接口、枚举、反射等特征。这些新增加的功能类似于.NET Framework，这使得开发ASP.NET AJAX应用变得容易维护，容易扩充。现在我们看看Microsoft AJAX Library是如何支持以上特征的。
## 1.类、成员和名字空间

　　在Microsoft AJAX Library中，所有的JavaScript类都继承自object(类似于.NET Framework库，都继承自object)，在ASP.NET AJAX应用中你可以运用面向对象的编程模式创建继承自Microsoft AJAX基类的对象和组件，类有四种成员：字段、属性、方法、事件。字段和属性是名/值对，用于描述一个类的一个实例的特性的。字段是由简单类型构成且可直接访问，例如：
~~~asp
　　myClassInstance.name="Fred"。
~~~
　　属性可以是任何简单类型或引用类型，通过get和set方法访问。在ASP.NET AJAX中，get和set是独立的函数，并规定在函数名中使用前缀"get_" 或 "set_" ，例如要获取或设置cancel属性的值时，你可以调用get_cancel或set_cancel方法。

　　一个方法是完成一个活动的函数而不是返回一个属性的值。属性和方法在下面的例子里都有示范。

　　事件指示特指的动作发生。当一个事件发生时，它可以调用一个或多个函数。事件所有者可以完成等待事件发生的任何任务。

　　名字空间是对关联类的逻辑分组。名字空间使你可以对公共功能进行分组。

　　为了使ASP.NET Web页面具有ASP.NET AJAX功能，你必须添加控件到页面上，当页面启动时，参照ASP.NET AJAX库的脚本自动产生。

　　下面的例子显示了页面使用了控件。
~~~asp
      < asp:ScriptManager runat="server" ID="scriptManager" />
~~~
　　下面的例子演示了如何使用Type.registerNamespace和.registerClass方法来把Person类增加到Demo名字空间中、创建类然后注册类。
　　
~~~asp
      Type.registerNamespace("Demo");
　　Demo.Person = function(firstName, lastName, emailAddress) {
　　this._firstName = firstName;

　　this._lastName = lastName;

　　this._emailAddress = emailAddress;

　　}

　　Demo.Person.prototype = {
　　getFirstName: function() {
　　return this._firstName;

　　},

　　getLastName: function() {
　　return this._lastName;

　　},

　　getName: function() {
　　return this._firstName + ' ' + this._lastName;

　　},

　　dispose: function() {
　　alert('bye ' + this.getName());

　　}

　　}

　　Demo.Person.registerClass('Demo.Person', null, Sys.IDisposable);
~~~
　在脚本文件Namespace.js中定义了类Person，制定了类的名字空间为"Demo"。运行页面Namespace.aspx，点击按钮将创建一个Demo.Person类的实例。
## 2.访问修饰

　　许多面向对象编程语言都有访问修饰的概念。允许你指定类或成员在某种范围内有效。例如可在外部执行的程序、具有相同名字空间的内部类或特指的代码快内的类等。在JavaScript中没有访问修饰，但在ASP.NET AJAX中约定以下划线字符开头"_"的被认为是私有的，类的外部不能访问。

## 3.继承

　　继承是一个类派生于另一个类的能力。派生类自动继承基类的所有字段、属性、方法和事件。派生类可以增加新的成员或者重写基类已存在的成员来改变成员的行为。

　　下面的脚本实例有两个类Person和Employee，Employee从Person继承而来，两个类示范了私有字段的使用，它们都有公共属性、方法。另外Employee类重写了Person类的toString实现，并调用了基类的功能。

　　
~~~asp
      Type.registerNamespace("Demo");
　　Demo.Person = function(firstName, lastName, emailAddress) {
　　this._firstName = firstName;

　　this._lastName = lastName;

　　this._emailAddress = emailAddress;

　　}

　　Demo.Person.prototype = {
　　getFirstName: function() {
　　return this._firstName;

　　},

　　getLastName: function() {
　　return this._lastName;

　　},

　　getEmailAddress: function() {
　　return this._emailAddress;

　　},

　　setEmailAddress: function(emailAddress) {
　　this._emailAddress = emailAddress;

　　},

　　getName: function() {
　　return this._firstName + ' ' + this._lastName;

　　},

　　dispose: function() {
　　alert('bye ' + this.getName());

　　},

　　sendMail: function() {
　　var emailAddress = this.getEmailAddress();

　　if (emailAddress.indexOf('@') < 0) {
　　emailAddress = emailAddress + '@example.com';

　　}

　　alert('Sending mail to ' + emailAddress + ' ...');

　　},

　　toString: function() {
　　return this.getName() + ' (' + this.getEmailAddress() + ')';

　　}

　　}

　　Demo.Person.registerClass('Demo.Person', null, Sys.IDisposable);

　　Demo.Employee = function(firstName, lastName, emailAddress, team, title) {
　　Demo.Employee.initializeBase(this, [firstName, lastName, emailAddress]);

　　this._team = team;

　　this._title = title;

　　}

　　Demo.Employee.prototype = {
　　getTeam: function() {
　　return this._team;

　　},

　　setTeam: function(team) {
　　this._team = team;

　　},

　　getTitle: function() {
　　return this._title;

　　},

　　setTitle: function(title) {
　　this._title = title;

　　},

　　toString: function() {
　　return Demo.Employee.callBaseMethod(this, 'toString') + '/r/n' + this.getTitle() + '/r/n' + this.getTeam();

　　}

　　}

　　Demo.Employee.registerClass('Demo.Employee', Demo.Person);
~~~
　Inheritance.js脚本文件中定义了两个类：Person和Employee，Employee是从Person继承而来。每个类都有字段、公共属性和方法。另外，Employee类重写了toString的实现，并在重写的代码中调用了基类的功能。在这个例子中把类Person的名字空间设定为"Demo"。运行页面Inheritance.aspx，点击“创建对象”、“对象释放”、“公共和私有属性”、“对象方法”、“重写方法”，“对象类型检查”体验一下。
## 4.接口

　　接口是类要实现的逻辑协议，是对类进行集成的公共遵守的规范。它能使多个类和同一个接口把实现定义和类的具体实现结合起来。下面的例子定义了一个基类Tree和接口IFruitTree，Apple和Banana这两个类实现了接口IFruitTree，但Pine类没有实现接口IFruitTree。

　　
~~~asp
      Type.registerNamespace("Demo.Trees");
　　Demo.Trees.IFruitTree = function() {}

　　Demo.Trees.IFruitTree.Prototype = {
　　bearFruit: function(){}

　　}

　　Demo.Trees.IFruitTree.registerInterface('Demo.Trees.IFruitTree');

　　Demo.Trees.Tree = function(name) {
　　this._name = name;

　　}

　　Demo.Trees.Tree.prototype = {
　　returnName: function() {
　　return this._name;

　　},

　　toStringCustom: function() {
　　return this.returnName();

　　},

　　makeLeaves: function() {}

　　}

　　Demo.Trees.Tree.registerClass('Demo.Trees.Tree');

　　Demo.Trees.FruitTree = function(name, description) {
　　Demo.Trees.FruitTree.initializeBase(this, [name]);

　　this._description = description;

　　}

　　Demo.Trees.FruitTree.prototype.bearFruit = function() {
　　return this._description;

　　}

　　Demo.Trees.FruitTree.registerClass('Demo.Trees.FruitTree', Demo.Trees.Tree, Demo.Trees.IFruitTree);

　　Demo.Trees.Apple = function() {
　　Demo.Trees.Apple.initializeBase(this, ['Apple', 'red and crunchy']);

　　}

　　Demo.Trees.Apple.prototype = {
　　makeLeaves: function() {
　　alert('Medium-sized and desiduous');

　　},

　　toStringCustom: function() {
　　return 'FruitTree ' + Demo.Trees.Apple.callBaseMethod(this, 'toStringCustom');

　　}

　　}

　　Demo.Trees.Apple.registerClass('Demo.Trees.Apple', Demo.Trees.FruitTree);

　　Demo.Trees.GrannySmith = function() {
　　Demo.Trees.GrannySmith.initializeBase(this);

　　// You must set the _description feild after initializeBase

　　// or you will get the base value.

　　this._description = 'green and sour';

　　}

　　Demo.Trees.GrannySmith.prototype.toStringCustom = function() {
　　return Demo.Trees.GrannySmith.callBaseMethod(this, 'toStringCustom') + ' ... its GrannySmith!';

　　}

　　Demo.Trees.GrannySmith.registerClass('Demo.Trees.GrannySmith', Demo.Trees.Apple);

　　Demo.Trees.Banana = function(description) {
　　Demo.Trees.Banana.initializeBase(this, ['Banana', 'yellow and squishy']);

　　}

　　Demo.Trees.Banana.prototype.makeLeaves = function() {
　　alert('Big and green');

　　}

　　Demo.Trees.Banana.registerClass('Demo.Trees.Banana', Demo.Trees.FruitTree);

　　Demo.Trees.Pine = function() {
　　Demo.Trees.Pine.initializeBase(this, ['Pine']);

　　}

　　Demo.Trees.Pine.prototype.makeLeaves = function() {
　　alert('Needles in clusters');

　　}

　　Demo.Trees.Pine.registerClass('Demo.Trees.Pine', Demo.Trees.Tree);
~~~
Interface.js脚本文件中定义了一个Tree基类和一个IFruitTree接口。Apple和Banana两个继承类实现了IFruitTree接口，而Pine类没有实现IFruitTree接口。运行Interface.aspx，点击“对象创建”、“接口检查”、“调用接口方法”体验一下。
## 5.枚举

　　枚举是包含一组被命名的正整数常数的类。你可以像访问属性一样访问它的值。例如：

　　myObject.color = myColorEnum.red，枚举提供了一种很容易理解的整数表示。下面的例子定义了一个以十六进制数表示的颜色被命名为有意义的名字的枚举类型。

　　
~~~asp
      Type.registerNamespace("Demo");
　　// Define an enumeration type and register it.

　　Demo.Color = function(){};

　　Demo.Color.prototype =

　　{
　　Red: 0xFF0000,

　　Blue: 0x0000FF,

　　Green: 0x00FF00,

　　White: 0xFFFFFF

　　}

　　Demo.Color.registerEnum("Demo.Color");
~~~
　　运行Enumeration.aspx，选择下拉框中的颜色，脚本程序把背景色设为选中的枚举类型Demo.Color中的颜色。
## 6.反射

　　反射用于检查一个运行期程序的结构和组成，是通过类Type的API来实现反射的，这些方法使你能够收集一个对象的信息，例如：它是继承于哪个类、它是否实现类某个特指的接口、它是否是某个类的实例等。

　　下面的例子用反射的API测试GrannySmith类是否实现了前面的接口。运行Reflection.aspx，点击“检查类型”、“检查继承”、“检查接口”体验一下。

　　另外需要说明的一点是，Microsoft AJAX Library是基于开放体系架构而开发的，不仅仅用于asp.net,还能用于其它体系架构中，例如用在java中。