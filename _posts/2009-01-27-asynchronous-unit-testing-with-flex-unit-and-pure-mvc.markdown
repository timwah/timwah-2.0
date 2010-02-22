---
layout: post
title: Asynchronous Unit Testing with Flex Unit and Pure MVC
disqus_url: http://timrobles.com/blog/entry/asynchronous_unit_testing_with_flex_unit_and_pure_mvc/
---

Testing asynchronous functions using [Flex Unit](http://opensource.adobe.com/wiki/display/flexunit/) is pretty straightforward using the addAsync method of TestCase.

{% highlight actionscript %}
var urlLoader:URLLoader  = new URLLoader();
urlLoader.addEventListener(Event.COMPLETE, addAsync(completeHandler, 5000));
urlLoader.load(new URLRequest("http://timrobles.com"));
{% endhighlight %}

But in the case of testing with <span class="autolink">Pure MVC</span>, you're most likely dealing with notifications instead of events. Say for example the above code actually makes a service request and your application is interested in listening for a custom ServiceRequest.SUCCESS notification rather than an Event.COMPLETE. This can be achieved by creating a proxy which wraps the Pure MVC notification system so it can be used with the addAsync call built into Flex Unit. The above code becomes (ServiceRequest is a dummy class):

{% highlight actionscript %}
var req:ServiceRequest  = new ServiceRequest(); 
// this class is like URLLoader but sends a ServiceRequest.SUCCESS 
// notification when the service call is complete
registerObserver(ServiceRequest.SUCCESS, requestCompleteHandler, 5000);
req.load(new URLRequest("http://timrobles.com");
{% endhighlight %}

The function "registerObserver" is a protected method of a NotificationTestCase class which does the wrapping for us. The original solution was posted here: [http://code.google.com/p/puremvc-flexunit-testing/](http://code.google.com/p/puremvc-flexunit-testing/) where you can download the source and see more examples. I have my own modified version of those classes, which includes a few helper methods like "removeObserver" and "removeAllObservers" so you can do some additional cleanup to ensure that your test cases run in isolation. You can grab the source for those here: [http://github.com/roblocop/trdotcom/tree/master/tests/puremvc](http://github.com/roblocop/trdotcom/tree/master/tests/puremvc).