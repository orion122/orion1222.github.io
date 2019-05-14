---
published: false
layout: post
title: Порядок присваиваний параметров в методах
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

Пример когда один из параметров имеет значение по-умолчанию:
{% highlight ruby %}
def args_unleashed(a,b=1,*c,d,e)
    puts "Arguments:"
    p a,b,c,d,e
end

#========irb session===========
irb(main):016:0> args_unleashed(1, 2, 3, 4, 5)
Arguments:
1
2
[3]
4
5
=> [1, 2, [3], 4, 5]
irb(main):017:0> args_unleashed(1, 2, 3, 4)
Arguments:
1
2
[]
3
4
=> [1, 2, [], 3, 4]
irb(main):018:0> args_unleashed(1, 2, 3)
Arguments:
1
1
[]
2
3
=> [1, 1, [], 2, 3]
irb(main):019:0> args_unleashed(1, 2, 3, 4, 5, 6, 7)
Arguments:
1
2
[3, 4, 5]
6
7
=> [1, 2, [3, 4, 5], 6, 7]
{% endhighlight %}

![Приоритет присваиваний]({{site.baseurl}}/assets/order-of-priority-assignments.png)

Параметры со значением по-умолчанию не могут находиться справа от параметра со звездочкой.
{% highlight console %}
irb(main):020:0> def broken_args(x, *y, z=1)
irb(main):021:1> end
SyntaxError: (irb):20: syntax error, unexpected '=', expecting ')'
def broken_args(x, *y, z=1)
                         ^
{% endhighlight %}