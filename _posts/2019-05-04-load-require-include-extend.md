---
published: false
layout: post
title: load и require. include и extend
---
### load и require
Метод load позволяет включить файл в другой файл, при этом загрузка этого файла, будет происходить каждый раз в момент вызова кода где указано подключение.

{% highlight ruby %}
# first.rb
puts 'Первый файл.'
load 'second.rb'
puts 'Снова первый файл.'
load 'second.rb'
{% endhighlight %}

{% highlight ruby %}
# second.rb
puts 'Второй файл.'
{% endhighlight %}

{% highlight console %}
$ ruby first.rb
Это первый файл.
Второй файл.
Снова первый файл.
Второй файл.
{% endhighlight %}

