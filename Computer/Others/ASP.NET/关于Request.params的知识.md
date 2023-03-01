<h1>关于Request.params的知识</h1>
**1、request.params怎么在两个页面传数据？**
request.params其实是一个集合，它依次包括request.querystring、request.form、request.cookies和request.servervariables。
如果要在两个页面传递数据的话，只能用request.querystring、request.form、request.cookies
Request.Params 是在 QueryString、Form、Server Variable 以及 Cookies 找数据，它首先在 QueryString 集合查找数据，如果在QueryString 找到数据，就返回数据，如果没有找到就去 Form 集合中查找数据，找到就返回，否则在往下一下个集合查找数据。 

**2、Request.Params["id"]、Request.Form["id"]和Request.QueryString["id"]的用法以及区别?**
Request.Params是所有post和get传过来的值的集合,Request.Form是取post传值, Request.QueryString是get传过来的值
