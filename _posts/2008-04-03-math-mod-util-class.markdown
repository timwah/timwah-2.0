---
layout: post
---

In most programming languages (or Actionscript at least), they use the computational definition of mod where it simply is the remainder of the first operand divided by the second. This works fine for n % m where n, m are elements of <b>N</b>. However, for -3 % 4, the computation looks something like this:
{% highlight actionscript %}
for n % m, n = -3, m = -4:
-3 / 4 = -.75
-.75 * 4 = -3
so -3 % 4 = -3
{% endhighlight %}
The mathematical definition of mod says for n mod m, where n is an element of <b>Z</b>, and m is an element of <b>N</b>, the result of n mod m is in the range [ 0, m - 1 ]. So -3 mod 4 = 1. This is a simple fix for Flash's % operator. The code below is in AS2, but the AS3 version is essentially the same (maybe you would ensure n, m are ints):
{% highlight actionscript %}
public static function mod(n:Number, m:Number):Number
{
    var tmp:Number = n % m;
    return tmp < 0 ? tmp + m : tmp;
}
{% endhighlight %}
This is great for implementing a circular array, say for a nav where you can press left + right keys and when you get to the first element and press left, you go to the last nav element ( n - 1 if n is the length ). So for your leftKeyHandler function you can just do something like this instead of checking if index == -1, index = length - 1:
{% highlight actionscript %}
function leftKeyHandler():Void
{
    selectNav(MathUtil.mod(--index, navElements.length)); 
}
{% endhighlight %}