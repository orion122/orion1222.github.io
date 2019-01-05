---
layout: post
title:  "Тренировочный pet-project #10. NATS"
---
После проверки ментора предстояло сделать следующее:
- Не отображать ненужные статусы сообщений (к примеру, статусы входящих сообщений)
- Прикрутить Rollbar
- Вынести header'ы в отдельное место
- Забирать с сервера не все сообщения чата каждый раз, а только diff-сообщений
- Добавить кнопку "Выход из чата". Если участник чата не вышел из чата, то в следующий раз при входе на главную страницу перекидывать его в чат.
- Прикрутить [NATS](https://nats.io/documentation/)


С первыми тремя заданиями справился быстро.


Проблема возникла с выходом из чата и nats.


При выходе из чата, после обновления страницы все равно перенаправляло в чат и только после второго выхода перенправления не происходило. Все дело в том, что токен сессии хранится в CDATA (//<![CDATA[
window.gon={};gon.session_token="a8597fac-a277-42cb-9b2a-36b51068bcda";
//]]>) и пока не обновится главная страница, токен сессии естественно не обновляется. После выхода главная страница не обновлялась, обновлялась только часть vue-темплэйта.
Пришлось добавить обновление страницы после выхода, чтобы токен сеcсии на главной странице обновился.


Diff-сообщений получать не пришлось, так как сообщения передавались по одному через websocket. А вот с публикацией сообщений с помощью nats возникла проблема. После первого запуска приложения первое сообщение после отправки отображается на экране мгновенно. Все следующие с задержкой примерно в минуту. Сообщения сразу сохраняются в базе, затем должны быть опубликованы nats'ом, но публикуются с задержкой.
После отправки сообщения не происходит мгновенного выхода из [этого](https://github.com/orion122/anonymous-chat/blob/nats/app/controllers/chats/messages_controller.rb#L51) цикла. Если в конец цикла добавлять NATS.stop как в документации, то сообщения не публикуются. Пробовал во Vue отписываться после получения сообщения и подписываться заново, но сообщения так же публикуются с задержкой.
Возможно проблема в [этом](https://github.com/isobit/ws-tcp-relay) приложении, но у него нет дебаг-режима.
Не смог разобраться с это проблемой.