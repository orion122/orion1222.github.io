---
published: true
layout: post
title: dup vs clone
---

В Ruby есть два способа сделать копию объекта: **dup** и **clone**.

Для чего может понадобиться копировать объект? Многие объекты в Ruby являются мутабельными, их можно изменять. Если нужно изменить объект, но сохранить копию оригинального, можно сделать копию объекта.

Если нужен массив со всеми его элементами кроме первого:

{% highlight ruby %}
a = [1,2,3,4,5]
a[1..-1]
# [2,3,4,5]
{% endhighlight %}

или

{% highlight ruby %}
b = a.clone
b.shift
# [1]
b
# [2,3,4,5]
{% endhighlight %}

Оба примера позволяют сохранит оригинальный массив.

![dup vs clone]({{site.baseurl}}/assets/dup-vs-clone.png)

#### Отличия
**dup** и **clone** не являются алиасами друг друга. Между ними есть несколько небольших различий. Отличие в том, что dup не копирует frozen-статус, tainted-статус, singleton-класс объекта.

{% highlight ruby %}
original = {
  :ruby => ["rails", "sinatra"],
  :erlang => ["chicago_boss", "nitrogen"]
}

# создаем уникальный (singleton) метод объекта original
def original.framework_names
  self.values.flatten
end

original.singleton_methods #=> [:framework_names]
original.framework_names
#=> ["rails", "sinatra", "chicago_boss", "nitrogen"]

# "замораживаем" объект (объект нельзя изменять)
original.freeze
original.frozen? #=> true

# делаем копию с использованием #dup
duplicate = original.dup
duplicate.frozen? #=> false
duplicate.singleton_methods #=> []

# делаем копию с использованием #clone
clone = original.clone
clone.frozen? #=> true
clone.singleton_methods #=> [:framework_names]
{% endhighlight %}

В Ruby 2.4 у clone появилась возможность игнорировать frozen-статус:

{% highlight ruby %}
a.clone(freeze: true)
a.clone(freeze: false)
{% endhighlight %}

#### Глубокая и поверхностная копия
**clone** и **dup** создают поверхностные копии объектов, то есть копируют объект, но не составляющие его объекты. Если у вас есть массив строк, будет скопирован только массив, а не сами строки.

{% highlight ruby %}
original = %w(apple orange banana)
copy     = original.clone
original.map(&:object_id)
# [23506500, 23506488, 23506476]
copy.map(&:object_id)
# [23506500, 23506488, 23506476]
{% endhighlight %}

Как видно, идентификаторы строк одинаковы даже после клонирования массива.

[Dup vs Clone in Ruby: Understanding The Differences](https://www.rubyguides.com/2018/11/dup-vs-clone/)

[Копирование объектов в Ruby](http://rubydev.ru/2012/09/03/object-copying-in-ruby-dup-clone/)
