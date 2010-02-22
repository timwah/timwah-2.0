---
layout: post
title: Using Enums in AS3
disqus_url: http://timrobles.com/blog/entry/using_enums_in_as3/
---

Using the enum strategy can save you some heartache if you want to create type safe applications. Say you have a type characteristic of an object. For example, every time you create a new Person(), you can specify a height like new Person("tall"), new Person("average") or new Person("short"). But how do you ensure that you aren't saying new Person("some height that isn't supported")? In the past what I've done, is create a separate class that only contains constants that are valid heights.
{% highlight actionscript %}
public class PersonHeight
{
    public static const TALL:String = "tall";
    public static const AVERAGE:String = "average";
    public static const SHORT:String = "short";
}
{% endhighlight %}
Although this allows me to change what the actual values of these constants are easily throughout the app, it still doesn't give me type safe checking to say whatever I pass into new Person(height:String) is valid or not. By simply modifying how we are specifying our height constants, we can do better than passing in a generic string.
{% highlight actionscript %}
public class PersonHeight
{
    public static const TALL:PersonHeight = new PersonHeight("tall");
    ...
    public var value:String;
    
    public function PersonHeight(value:String)
    {
         this.value = value;
    }
}
{% endhighlight %}
Now in my person class, I can use the PersonHeight as a type and not just as a container for constants. 
{% highlight actionscript %}
new Person(height:PersonHeight)
{% endhighlight %}
Et voila! A type safe parameter. Obviously this is only the tip of the iceberg, but since this is a recent discovery for me, I think it's awesome and allows me to geek out even more than I normally would ;)