---
published: false
layout: post
title: Порядок параметров и аргументов
---
Аргументы можно "свернуть" с помощью звёздочки — тогда метод получит массив в качестве аргумента:

{% highlight ruby %}
def sum(*members)
    members[0] + members[1]
end

sum(10, 2) #=> 12
{% endhighlight %}

Другой пример:

{% highlight ruby %}
def mixed_args(a,b,*c,d)
    puts "Arguments:"
    puts a,b,c,d
end

mixed_args(1,2,3,4,5)

#Arguments:
#1
#2
#[3, 4]
#5
{% endhighlight %}

Ruby пытается присвоить значения как можно большему количеству переменных. Параметр со звездочкой имеет низший приоритет. В примере выше обязательные параметры (a, b, d) обрабатываются до *c. Если передать количество аргументов достоточное только для обязательных аргументов, то массив c будет пустым. Ему ничего не достанется :)

{% highlight ruby %}
mixed_args(1,2,3)

#Arguments:
#1
#2
#[]
#3
{% endhighlight %}