---
layout: post
title:  "Читаемость кода"
---
По работе нужно было написать скрипт, который периодически запускается и проверяет доступность основного сервера и в случае если он не доступен, меняет маршрут вышестоящего узла так, чтобы трафик пошел через резервный.
На бумаге словами записал логику работы:
> Если сейчас доступен, а до этого не был доступен, то переключить на основной,
> если сейчас не доступен, а до этого был доступен, то переключить на резервный

Долго сидел над кодом, пока не дал функциям и переменным хорошие названия.
В итоге код стал похож на человеческую речь. Приведу код соответствующий записанному на бумаге:
{% highlight python %}if isAvailableNow and not wasAvailableBefore:
    save_availability_state(isAvailableNow)
    switch_main_cisco_to_primary()
elif not isAvailableNow and wasAvailableBefore:
    save_availability_state(isAvailableNow)
    switch_main_cisco_to_secondary(){% endhighlight %}

Легко читается, но тут дублируется `save_availability_state()`.

В итоге добавил функцию:
{% highlight python %}def isStateChanged(isAvailableNow, isAvailableBefore):
    return isAvailableNow != isAvailableBefore{% endhighlight %}

и переписал так:
{% highlight python %}if isStateChanged(isAvailableNow, wasAvailableBefore):
    save_availability_state(isAvailableNow)
    switch_main_cisco_to_primary() if isAvailableNow else switch_main_cisco_to_secondary(){% endhighlight %}
Хотя, конечно, вложенность получилась больше, но по-моему так читается еще легче.
