---
layout: post
title:  "Тренировочный pet-project #4"
---
Создал ветку для экспериментов. Теперь токен сессии создается не сразу на главной странице, а после создания чата или после присоединения к чату. При редиректе на чат (после создания или после присоединения к чату), в query string передается токен сессии. Это необходимо чтобы в `chats.coffee` он был известен, так как оттуда передается в `chat_channel`, где затем отправленнные сообщения сохраняются в БД с привязкой к сессии.

Понял, что модели и миграции неправильные. Долго искал свой или похожий вариант [отношений](http://rusrails.ru/active-record-associations). Выбирал между `has_many :through` и `has_and_belongs_to_many`. Только когда нарисовал рисунок
```
________chat_________
| session1: message1 |
| session2: message1 |
| session1: message2 |
|____________________|
```
понял, что на самом деле у меня очень простой вариант: `Chat has_many :sesions`, `Session has_many :messages`.

Исправил тесты в экспериментальной ветке.

По совету ментора воспользовался гемом [gon](https://github.com/gazay/gon), для упрощенной передачи данных из контроллеров в js-файлы.

Осталось разобраться как в js-файле получать данные из БД.