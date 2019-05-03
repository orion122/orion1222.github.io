---
published: false
---
## Различия между puts, print и p

**puts** в отличие от **print** добавляет новую строку в конце:
{% highlight ruby %}
puts 123
puts 456
puts 789
123
456
789
{% endhighlight %}

{% highlight ruby %}
print 123
print 456
print 789
123456789
{% endhighlight %}


**puts** в отличие от **print** распечатывает элементы массива по одному в строке:
{% highlight ruby %}
puts [1,2]
1
2
{% endhighlight %}

{% highlight ruby %}
print [1,2]
[1,2]
{% endhighlight %}


**puts** пытается преобразовать все в строку.
{% highlight ruby %}
puts [1,nil,nil,2]
1


2
{% endhighlight %}


**p** - метод, который показывает более "сырую" версию объекта. Аналогичен puts объект.inspect.
{% highlight ruby %}
puts "Ruby Is Cool"
Ruby Is Cool

p "Ruby Is Cool"
"Ruby Is Cool"
{% endhighlight %}


**puts** всегда возвращает nil, **p** возвращает переданный ему объект

![]({{site.baseurl}}/https://i2.wp.com/www.rubyguides.com/wp-content/uploads/2018/10/ruby-print-puts-mindmap.png?w=891&ssl=1)

[Источник](https://www.rubyguides.com/2018/10/puts-vs-print/ "rubyguides.com")