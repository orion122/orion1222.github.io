---
layout: post
title:  "Тренировочный pet-project #8"
---
В этот раз было много мелких замечаний:
- заменить `{...}` на `do ... end`
- удалять закомментированные строки кода
- удалить бессмысленные состояния сообщений ('отправлено, неотправлено')
- заменить `urlsafe_base64` на `uuid`
- `map` заменить на `find_each`
- вернуть `index unique` для токена сессии


Несколько чуть посложней:
- вернуть gon и перенести сохранение токена сессии в cookie в JS
- возвращать http-статусы вместо json
- установить `rubocop` и проверить код
- заменить `before` на `subject` в тестах там где можно
- сделать как получается статус 'прочитано'


Одно сложное - разобраться с правильной иерархией контроллеров.


Немного проблем возникло со статусом 'прочитано' и с тем, что связано с иерархией контроллеров.
Статус 'прочитано' сделал. Решение мне не нравится, но другого так и не придумал.
С иерархией контроллеров в итоге вроде разобрался:
- перенес в папку chats контроллер messages в namespace'е Chats
- в папке chats создал application_controller в namespace'е Chats
- в application_controller создал методы session_token и chat (теперь они доступны в контроллере messages и нет дублирования кода)
- подправил роуты так чтобы был доступен контроллер messages (`scope module: :chats`)


Полезные ссылки:
- [Иерархия контроллеров](https://habr.com/post/136461/)
- [Rubocop](https://github.com/bbatsov/rubocop)
- [Rubocop configuration](https://github.com/bbatsov/rubocop/blob/master/manual/configuration.md)
- [Rspec subject](https://relishapp.com/rspec/rspec-core/docs/subject)
- [Better specs (subject)](http://www.betterspecs.org/#subject)
- [Rails 5 Routes: Scope vs Namespace](https://devblast.com/b/rails-5-routes-scope-vs-namespace)
- [How Do Batched Queries Work?](http://www.monkeyandcrow.com/blog/reading_rails_how_do_batched_queries_work/)
