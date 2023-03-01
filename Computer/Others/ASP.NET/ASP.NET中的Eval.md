<h1>ASP.NET中的Eval</h1>
ASP.NET中的Eval方法实际上是属于TemplateControl的，而System.Web.UI.Page和System.Web.UI.UserControl都继承于TemplateControl，所以我们可以在Page和UserControl上可以直接调用个方法。
Page.Eval方法可以帮助我们更好的撰写数据绑定表达式，在ASP.NET 1.x时代，数据绑定表达式的一般形式是：
~~~asp
<%# DataBinder.Eval( Container , “DataItem.Name”) %>
~~~
而在ASP.NET 2.0中，同样的代码，我们可以这样写：
~~~asp
<%# Eval( “Name” )%>
~~~
ASP.NET 2.0是怎么实现的呢？我们先从Eval方法来研究，通过反射.NET Framework 2.0类库的源代码，我们可以看到这个方法是这样实现的：
~~~asp
protected internal object Eval(string expression)
{
    this.CheckPageExists();
    return DataBinder.Eval(this.Page.GetDataItem(), expression);
}
~~~
第一行我们不必管，这是检查调用的时候有没有Page对象的，如果没有则会抛出一个异常。
关键是第二行：
~~~asp
return DataBinder.Eval(this.Page.GetDataItem(), expression);
~~~
Page.GetDataItem()也是2.0中新增的一个方法，用途是正是取代ASP.NET 1.x中的Container.DataItem。
看来不摸清楚GetDataItem()方法，我们也很难明白Eval的原理。GetDataItem的实现也很简单：
~~~asp
public object GetDataItem()
{
    if ((this._dataBindingContext == null) || (this._dataBindingContext.Count == 0))
    {
        throw new InvalidOperationException(SR.GetString("Page_MissingDataBindingContext"));
    }
    return this._dataBindingContext.Peek();
}
~~~
我们注意到了有一个内部对象_dataBindingContext，通过查源代码发现这是一个Stack类型的东西。所以他有Peek方法。而这一段代码很容易看懂，先判断这个Stack是否被实例化，然后，判断这个Stack里面是不是有任何元素，如果Stack没有被实例化或者没有元素则抛出一个异常。最后是将这个堆栈顶部的元素返回。
ASP.NET 2.0用了一个Stack来保存所谓的DataItem，我们很快就查到了为这个堆栈压元素和弹出元素的方法：Control.DataBind方法：
~~~asp
protected virtual void DataBind(bool raiseOnDataBinding)
{
    bool flag1 = false;//这个标志的用处在上下文中很容易推出来，如果有DataItem压栈，则在后面出栈。
    if (this.IsBindingContainer)//判断控件是不是数据绑定容器，实际上就是判断控件类是不是实现了INamingContainer
    {
        bool flag2;
        object obj1 = DataBinder.GetDataItem(this, out flag2);//这个方法是判断控件是不是有DataItem属性，并把它取出来。
        if (flag2 && (this.Page != null))//如果控件有DataItem
        {
            this.Page.PushDataBindingContext(obj1);//把DataItem压栈，PushDataBindingContext就是调用_dataBindingContext的Push方法
            flag1 = true;
        }
    }
    try
    {
        if (raiseOnDataBinding)//这里是判断是不是触发DataBinding事件的。
        {
            this.OnDataBinding(EventArgs.Empty);
        }
        this.DataBindChildren();//对子控件进行数据绑定，如果这个控件有DataItem，则上面会将DataItem压入栈顶，这样，在子控件里面调用Eval或者GetDataItem方法，就会把刚刚压进去的DataItem给取出来。
    }
    finally
    {
        if (flag1)//如果刚才有压栈，则现在弹出来。
        {
        this.Page.PopDataBindingContext();//PopDataBindingContext就是调用_dataBindingContext的Pop方法
        }
    }
}

至此，我们已经可以完全了解ASP.NET 2.0中GetDataIten和Eval方法运作的原理了