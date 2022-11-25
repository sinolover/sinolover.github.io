**1. tomcat启动后work目录下是空，访问网站，报404错误**
tomcat下面删除work目录，清空temp目录

**2. JavaEE项目发布时，报找不到类错误**
  Eclipse  Project->clean

**3. 在使用eclipse发布到tomcat时提示Java.lang.ClassNotFoundException: org.springframework.web.context.ContextLoaderListener**
>感觉很奇怪，于是到网站发布目录发现在WEB-INF里面没有lib目录，这就是为什么提示类没有找到的原因，导致该问题可以按照下面方法解决：
+ 右键点击项目，选择properties选项
+ 选择“Deployment Assembly”项，选择“Add” “”Java Build Path Entries”并选择“”Maven Depedencies”选项即可
+ 重新发布就会成功

**4. Tomcat version 6.0 only supports J2EE 1.2, 1.3, 1.4, and Java EE 5 Web modules**
用eclipse做项目，新建项目时什么都用最新的版本，在Dynamic web module version栏里选了最新的3.0版本，布署项目的时候就出现了Tomcat version 6.0 only supports J2EE 1.2, 1.3, 1.4, and Java EE 5 Web modules错误

解决方法：
+ project的.setting folder下面，有个名为org.eclipse.wst.common.project.facet.core.xml的文件，里面配置有各种版本信息。此时，按照本机配置修改这个文件，问题就解决了。
~~~cpp
<?xml version="1.0" encoding="UTF-8"?>
<faceted-project>
<runtime name="Apache Tomcat v5.5"/>
<fixed facet="jst.web"/>
<fixed facet="jst.Java"/>
<installed facet="jst.java" version="5.0"/>
<installed facet="jst.web" version="3.0"/>
<installed facet="wst.jsdt.web" version="1.0"/>
</faceted-project>
~~~

+ 可以下载Tomcat 7.0解决
+ 可以在配置文件中把
~~~cpp
<installed facet="jst.web" version="3.0"/> 
~~~
改成低些的版本version="2.5"
