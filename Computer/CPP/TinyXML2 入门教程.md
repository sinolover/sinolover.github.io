​

 转自：[TinyXML2 入门教程\_恋喵大鲤鱼的博客-CSDN博客\_tinyxml2中文指南](https://blog.csdn.net/K346K346/article/details/48750417 "TinyXML2 入门教程_恋喵大鲤鱼的博客-CSDN博客_tinyxml2中文指南")

代码编译运行环境：Linux 64bits + Debug + g++ -m64（-m 表示生成 64bits 的程序）

# 1. TinyXML2 概述

TinyXML2 是 simple、small、efficient 的开源 C++ XML 文件解析库，可以很方便地应用到现有的项目中。

TinyXML2 详细介绍与源码获取详见：[TinyXML2 官网](http://www.grinninglizard.com/tinyxml2/index.html "TinyXML2 官网")。

# 2. TinyXML1 与 TinyXML2 对比

TinyXML1 与 TinyXML2 这两个著名的开源 XML 文件解析库均出自 [Lee Thomason](https://github.com/leethomason "Lee Thomason") 之手，让我们向这位满怀开源精神的大家致敬。

TinyXML1 开发已经停止，所有的开发都转移到了 TinyXML2。TinyXML2 适用于大部分 C/C++ 的项目，经得住考验，是最好的选择。较 TinyXML1 而言，TinyXML2 化繁为简，使用时只需要包含两个文件，而 TinyXML1 需要包含 6 个文件，一般生成静态链接库供项目使用。TinyXML1 详细介绍与源码见：[TinyXML1官网](http://www.grinninglizard.com/tinyxml/ "TinyXML1官网")。TinyXML1 用法用例可以参考博文：[TinyXML快速入门](https://www.cnblogs.com/clever101/tag/TinyXml%E5%BA%93%E4%BD%BF%E7%94%A8/ "TinyXML快速入门")。

TinyXML2 使用了与 TinyXML1 相似的 API，并且拥有丰富的测试案例。但 TinyXML2 解析器相对 TinyXML1 在代码上是完全重写，使其更适合于游戏开发。它使用更少的内存，效率更高。

TinyXML2 无需 STL，也放弃了对 STL 支持。所有字符串查询均使用 C 风格字符串`const char*`来表示，省去 string 类型对象的构造，使代码更加简洁。

**二者共同点**：  
（1）都使用了简单易用的 API。  
（2）都是基于 DOM（Document Object Model，文档对象模型）的解析器。  
（3）都支持 UTF-8 编码。

**TinyXML2 的优点： ** 
（1）对大部分大部分的 C/C++ 项目具有普适性；  
（2）使用较少的内存（约 TinyXML1 的 40%），速度更快；  
（3）没有 C++ 的 STL 的要求；  
（4）更接近现代 C++ 的特性，如使用了适当的命名空间；  
（5）适当有效的处理了的空白字符（空格，Tab 和回车）。

**TinyXML1 的优点：  **
（1）可以报告分析错误的位置；  
（2）提供一些 C++ STL 公约支持：流和字符串；  
（3）拥有非常成熟和良好的调试代码库。

# 3. TinyXML2 用法用例

TinyXML2 的网上教程并不多见，醍醐灌顶、受益匪浅的教程更是凤毛麟角。有的也是蜻蜓点水、参差不齐的泛泛而谈。最终，所能参考的资料也就是官网文档和示例代码，但却有点晦涩难懂。因此，本文就为了解决这个尴尬的局面，结合官网资料和网上资源，尽量详细地列出 TinyXML2 的常见用法用例，不足之处，请留言补充，后续增加修改。

XML 文件本质就是小型的数据库，换个角度来说就是，对数据库有什么操作，那么对 XML 文件就应能实现什么操作。一般而言，对数据库的操作包括以下几种：新建数据库和对数据库增删查改。那么对应 XML 文件就是新建 XML 文件、增加 XML 文件的节点，删除XML文件的指定节点，查询 XML 文件指定节点的值，修改 XML 文件节点的值。

使用方法：将 tinyxml2.cpp 和 tinyxml2.h 拷贝至项目目录，使用时包含头文件和引入名字空间。

```
#include "tinyxml2.h"
using namespace tinyxml2
```

使用场景：存储用户信息。

用户数据表设计如下：

| 变量名 | 描述 | 类型 | 长度（字节） | 不为空 | 主键 |
| --- | --- | --- | --- | --- | --- |
| UserName | 用户名 | Vchar | 3-20 | Y | Y |
| Password | 密码 | Char | 32 | Y | N |
| Gender | 性别 | Int | 1 | N | N |
| Mobile | 电话 | Char | 11 | N | N |
| Email | 电子邮箱 | Varchar | 1-50 | N | N |

对应 XML 文件实现如下：

```
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<DBUSER>
	<User Name=”lvlv” Password =”123456”>
		<Gender></Gender>
		<Mobile ></ Mobile>
		<Email ></ Email >
	</User>
	<!-- This is a comment --> 
<DBUSER>
```

从中可以看出，XML 由三大部分组成，分别是声明、根节点和其它节点。其中 XML 文件的声明包括三方面的内容：Version、Standalone 和 Encoding。下面将详细列出常见 TinyXML2 的用法。

注意：以下示例代码针对本人下载使用的 TinyXML2，官网的 TinyXML2 在不断地完善和更新当中，最新的 TinyXML2 和本文的示例代码可能会有出入。本人使用的 TinyXML2 是 2015.9.23 从官网下载的，已上传至 CSDN，下载见：[TinyXML2](http://download.csdn.net/download/k346k346/9140753 "TinyXML2")。

## 3.1 创建 XML 文件

示例代码：

```
//function：create a xml file
//param：xmlPath:xml文件路径
//return：0,成功；非0，失败
int createXML(const char* xmlPath)
{
	const char* declaration ="<?xml version=\"1.0\" encoding=\"UTF-8\" standalone=\"no\"?>";
	XMLDocument doc;
	doc.Parse(declaration);//会覆盖xml所有内容
	
	//添加申明可以使用如下两行
	//XMLDeclaration* declaration=doc.NewDeclaration();
	//doc.InsertFirstChild(declaration);

	XMLElement* root=doc.NewElement("DBUSER");
	doc.InsertEndChild(root);

	return doc.SaveFile(xmlPath);
}
```

创建结果：

```
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<DBUSER/>
```


## 3.2 增加 XML 文件的节点

示例代码：

```
//用户类
class User
{
public:
	User()
	{
		gender=0;
	};
	
	User(const string& userName, const string& password, int gender, const string& mobile, const string& email){
		this->userName=userName;
		this->password=password;
		this->gender=gender;
		this->mobile=mobile;
		this->email=email;
	};
	
	string userName;
	string password;
	int gender;
	string mobile;
	string email;
};

//function：insert XML node
//param：xmlPath xml 文件路径; user 用户对象
//return：0 成功; 非 0 失败
int insertXMLNode(const char* xmlPath,const User& user) {
	XMLDocument doc;
	int res=doc.LoadFile(xmlPath);
	if(res!=0)
	{
		cout<<"load xml file failed"<<endl;
		return res;
	}
	XMLElement* root=doc.RootElement();
	
	XMLElement* userNode = doc.NewElement("User");
	userNode->SetAttribute("Name", user.userName.c_str());
	userNode->SetAttribute("Password", user.password.c_str());
	root->InsertEndChild(userNode);
	
	XMLElement* gender = doc.NewElement("Gender");
	XMLText* genderText=doc.NewText(itoa(user.gender));
	gender->InsertEndChild(genderText);
	userNode->InsertEndChild(gender);
					
	XMLElement* mobile = doc.NewElement("Mobile");
	mobile->InsertEndChild(doc.NewText(user.mobile.c_str()));
	userNode->InsertEndChild(mobile);
	
	XMLElement* email = doc.NewElement("Email");
	email->InsertEndChild(doc.NewText(user.email.c_str()));
	userNode->InsertEndChild(email);
	
	return doc.SaveFile(xmlPath);
}
```

创建结果：

```
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<DBUSER>
    <User Name="lvlv" Password ="123456">
        <Gender>1</Gender>
        <Mobile>15813354926</Mobile>
        <Email>1589276509@qq.com</Email>
    </User>
</DBUSER>
```

## 3.3 查询 XML 文件的指定节点

Xml文件中，一个用户节点存储一个用户的信息。因此，对用户信息的增删查改，即无论查询节点、删除节点、修改节点和增加节点，都需要获取需要操作的节点。那么先实现一个根据用户名获取节点指针的函数：

```
//function:根据用户名获取用户节点
//param:root:xml文件根节点；userName：用户名
//return：用户节点
XMLElement* queryUserNodeByName(XMLElement* root,const string& userName) {
	XMLElement* userNode=root->FirstChildElement("User");
	while(userNode!=NULL) {
		if(userNode->Attribute("Name")==userName)
			break;
		userNode=userNode->NextSiblingElement();//下一个兄弟节点
	}
	return userNode;
}
```

在以上函数的基础上，获取用户信息的函数：

```
User* queryUserByName(const char* xmlPath,const string& userName) {
	XMLDocument doc;
	if(doc.LoadFile(xmlPath)!=0) {
		cout<<"load xml file failed"<<endl;
		return NULL;
	}
	XMLElement* root=doc.RootElement();
	XMLElement* userNode=queryUserNodeByName(root,userName);
	
	if(userNode!=NULL) {  //searched successfully 
		User* user=new User();
		user->userName=userName;
		user->password=userNode->Attribute("Password");
		XMLElement* genderNode=userNode->FirstChildElement("Gender");
		user->gender=atoi(genderNode->GetText());
		XMLElement* mobileNode=userNode->FirstChildElement("Mobile");
		user->mobile=mobileNode->GetText();		
		XMLElement* emailNode=userNode->FirstChildElement("Email");
		user->email=emailNode->GetText();			
		return user;
	}
	return NULL;
}
```

## 3.4 修改 XML 文件的指定节点

```
//function:修改指定节点内容
//param:xmlPath:xml文件路径；user：用户对象
//return：bool
bool updateUser(const char* xmlPath,User* user) {
	XMLDocument doc;
	if(doc.LoadFile(xmlPath)!=0) {
		cout<<"load xml file failed"<<endl;
		return false;
	}
	XMLElement* root=doc.RootElement();
	XMLElement* userNode=queryUserNodeByName(root,user->userName);
	
	if(userNode!=NULL) {
		if(user->password!=userNode->Attribute("Password")) {
			userNode->SetAttribute("Password",user->password.c_str());  //修改属性
		}
		XMLElement* genderNode=userNode->FirstChildElement("Gender");
		if(user->gender!=atoi(genderNode->GetText())) {
			genderNode->SetText(itoa(user->gender).c_str());   //修改节点内容
		}
		XMLElement* mobileNode=userNode->FirstChildElement("Mobile");
		if(user->mobile!=mobileNode->GetText()) {
			mobileNode->SetText(user->mobile.c_str());
		}
		XMLElement* emailNode=userNode->FirstChildElement("Email");
		if(user->email!=emailNode->GetText()) {
			emailNode->SetText(user->email.c_str());
		}
		if(doc.SaveFile(xmlPath)==0)
			return true;
	}
	return false;
}
```


验证代码：

```
int main(int argc,char* argv[]) {
	//修改用户信息
	User user("lvlv","00001111",0,"13995648666","1586666@qq.com");
	if(updateUser("./user.xml",&user))
		cout<<"update successfully"<<endl;
	else
		cout<<"update failed"<<endl;
	return 0;
}
```


修改结果：

```
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<DBUSER>
    <User Name="lvlv" Password="00001111">
        <Gender>0</Gender>
        <Mobile>13995648666</Mobile>
        <Email>1586666@qq.com</Email>
</User>
</DBUSER>
```


## 3.5 删除 XML 文件的指定节点内容

```
//function:删除指定节点内容
//param:xmlPath:xml文件路径；userName：用户名称
//return：bool
bool deleteUserByName(const char* xmlPath,const string& userName) {
	XMLDocument doc;
	if(doc.LoadFile(xmlPath)!=0) {
		cout<<"load xml file failed"<<endl;
		return false;
	}
	XMLElement* root=doc.RootElement();
	//doc.DeleteNode(root);//删除xml所有节点
	XMLElement* userNode=queryUserNodeByName(root,userName);
	if(userNode != NULL) {
		userNode->DeleteAttribute("Password");//删除属性
		XMLElement* emailNode=userNode->FirstChildElement("Email");
		userNode->DeleteChild(emailNode); //删除指定节点
		//userNode->DeleteChildren();//删除节点的所有孩子节点
		if(doc.SaveFile(xmlPath)==0)
			return true;
	}
	return false;
}
```

验证代码：

```
int main(int argc, char* argv[]) {
	//删除用户某些信息
	if(deleteUserByName("./user.xml","lvlv"))
		cout<<"delete successfully"<<endl;
	else
		cout<<"delete failed"<<endl;
	return 0;
}
```


删除结果：

```
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<DBUSER>
    <User Name="lvlv">
        <Gender>10</Gender>
        <Mobile>13995648666</Mobile>
</User>
</DBUSER>
```

# 4.其它常见用例

## 4.1 获取 XML 文件申明

```
//function:获取xml文件申明
//param:xmlPath:xml文件路径；strDecl：xml申明
//return：bool
bool getXMLDeclaration(const char* xmlPath,string& strDecl) {
	XMLDocument doc;
	if(doc.LoadFile(xmlPath)!=0) {
		cout<<"load xml file failed"<<endl;
		return false;
	}
	XMLNode* decl=doc.FirstChild();  
	if (NULL!=decl) {  
        XMLDeclaration* declaration =decl->ToDeclaration();  
        if (NULL!=declaration)  
        {  
              strDecl = declaration->Value();
			  return true;
        } 
    }
   	return false;
}
```

验证代码：

```
int main(int argc,char* argv[]) {
	//获取xml文件申明
	string strDecl;
	if(getXMLDeclaration("./user.xml",strDecl))
	{
		cout<<"declaration:"<<strDecl<<endl;
	}
	return 0;
}
```


验证结果：

```
declaration:xml version="1.0" encoding="UTF-8" standalone="no"
```

## 4.2 打印 XML 文件至标准输出

```
//function:将xml文件内容输出到标准输出
//param:xmlPath:xml文件路径
void print(const char* xmlPath) {
	XMLDocument doc;
	if(doc.LoadFile("./user.xml")!=0) {
		cout<<"load xml file failed"<<endl;
		return;
	}
	doc.Print();
}
```


## 4.3 XML 文件内容输出至内存

```
XMLPrinter printer;
doc.Print( &printer );
// printer.CStr() has a const char* to the XML
```


其它实例待以后用到再补充。


# 参考文献

[TinyXML2 官网](http://www.grinninglizard.com/tinyxml2/index.html "TinyXML2 官网")  
[leethomason github.TinyXML2](https://github.com/leethomason/tinyxml2 "leethomason github.TinyXML2")

​