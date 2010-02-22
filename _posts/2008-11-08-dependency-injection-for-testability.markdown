---
layout: post
disqus_url: http://timrobles.com/blog/entry/dependency_injection_for_testability/
---

My latest venture in improving my code has been incorporating the concept of <span class="autolink">Dependency Injection</span>. In general, the overarching idea has to do with the <span class="autolink">the law of demeter</span>. That is, the objects of my application should only collaborate or call actions on objects they use directly. For example, rather than passing a reference to a "service locator" or more commonly a shell or application class in Actionscript:
{% highlight actionscript %}
protected var loader:ILoader;
LoadedSWF(app:IApp)
{
    loader = app.getLoader();
}
{% endhighlight %}
I would say something like this instead:
{% highlight actionscript %}
protected var loader:ILoader;
LoadedSWF(loader:ILoader)
{
    this.loader = loader;
    ...
    (maybe queue up some thumbnails at this point)
}
{% endhighlight %}
The end goal is to have an object which only requires it's collaborators.  There's no reason for this loaded swf to know about the app at large; suppose we want to drop it into another application context that simply isn't built within our "app framework?"

<span class="autolink">Miško Hevery</span>
recently gave a talk on this, which I've linked here at the end of the post. Some great points to note:

* Object instantiation should be abstracted to factories to wire up your application. You can group factory logic together based on what the object lifetimes are of what they create. You might create a factory for compile time objects, runtime (usually loaded swfs) or service lifetime. With each case you only want to inject an object in the constructor that has a lifetime greater than or equal to the injectee.
* One of the best examples given is when you're purchasing something, do you give the clerk your wallet or the cash? In the same way, you should give your objects what they need directly instead of massaging data.</li>

Here are the tubes:

<object width="425" height="344"><param name="movie" value="http://www.youtube.com/v/RlfLCWKxHJ0&hl=en&fs=1&rel=0&color1=0x3a3a3a&color2=0x999999"></param><param name="allowFullScreen" value="true"></param><param name="allowscriptaccess" value="always"></param><embed src="http://www.youtube.com/v/RlfLCWKxHJ0&hl=en&fs=1&rel=0&color1=0x3a3a3a&color2=0x999999" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="425" height="344"></embed></object>

I'll be posting more on this topic later as it relates specifically to Actionscript.