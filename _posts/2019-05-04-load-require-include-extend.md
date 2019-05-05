---
published: true
layout: post
title: 'load, require, require_relative'
---
В Ruby есть три метода, которые могут быть использованы для загрузки: **load**, **reuqire** и **require_relative**.

### load
**load** обычно используется для загрузки произвольного файла, **require** и **require_relative** - для загрузки библиотеки.

Метод **load** позволяет включить файл в другой файл, при этом загрузка этого файла, будет происходить каждый раз в момент вызова кода где указано подключение. Это полезно для разработки, когда нужно видеть результат изменений вживую (находясь в сеансе irb и редактируя файл). Кроме того, расширение не может быть опущено: всегда должно указываться полное имя файла.

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

Если загружаемый файл находится в текущем каталоге, Ruby сможет найти его по имени.

{% highlight console %}
$ ruby first.rb
Это первый файл.
Второй файл.
Снова первый файл.
Второй файл.
{% endhighlight %}

Если не нашел загружаемый файл в текущем каталоге, то он будет искать его в _пути загрузки_.

### Путь загрузки ($LOAD_PATH)
В Ruby есть предопределенная глобальная переменная **$LOAD_PATH** (синоним **$:**). **$LOAD_PATH** представляет собой массив, в котором содержаться имена каталогов и в котором при загрузке файлов производят поиск методы **load** и **require**.

{% highlight console %}
$ ruby -e 'puts $LOAD_PATH'
/home/eq/.rbenv/rbenv.d/exec/gem-rehash
/home/eq/.rbenv/versions/2.4.5/lib/ruby/gems/2.4.0/gems/did_you_mean-1.1.0/lib
/home/eq/.rbenv/versions/2.4.5/lib/ruby/site_ruby/2.4.0
/home/eq/.rbenv/versions/2.4.5/lib/ruby/site_ruby/2.4.0/x86_64-linux
/home/eq/.rbenv/versions/2.4.5/lib/ruby/site_ruby
/home/eq/.rbenv/versions/2.4.5/lib/ruby/vendor_ruby/2.4.0
/home/eq/.rbenv/versions/2.4.5/lib/ruby/vendor_ruby/2.4.0/x86_64-linux
/home/eq/.rbenv/versions/2.4.5/lib/ruby/vendor_ruby
/home/eq/.rbenv/versions/2.4.5/lib/ruby/2.4.0
/home/eq/.rbenv/versions/2.4.5/lib/ruby/2.4.0/x86_64-linux
{% endhighlight %}

### require
Другой метод загрузки, **require**, также производит поиск в каталогах из **$LOAD_PATH**. Но **require** работает по-другому. При его использовании, подключаемый файл, загружается единоразово в память, и используется в дальнейшем из памяти. Но при разработке, придется постоянно перезапукать программу, чтобы видеть изменения. Расширение файла указывать необязательно. Если расширение не указано, то **require** сначала пытается найти файл с рашсирением `.rb`, если такой не найден, то с `.so`.

{% highlight ruby %}
# first.rb
puts 'Первый файл.'
require './second'
puts 'Снова первый файл.'
require './second'
{% endhighlight %}

{% highlight ruby %}
# second.rb
puts 'Второй файл.'
{% endhighlight %}

{% highlight console %}
$ ruby first.rb
Первый файл.
Второй файл.
Снова первый файл.
{% endhighlight %}

### require_relative
Третий способ загрузки файлов - **require_relative**. Этот метод производит поиск относительно каталога, в котором находится файл, из которого он вызван. Таким образом, в предыдущем примере можно было написать `require_relative 'second'`.