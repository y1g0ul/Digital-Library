---
created-dt: 2026-06-18 19:39
tags:
  - review
sr-due: 2026-06-22
sr-interval: 3
sr-ease: 250
---
Служба в [[Linux]] для отправки, получения и маршрутизации электронной почты (Mail Transfer Agent, MTA)
``` bash
postfix [команда]
```
Главный конфиг `/etc/postfix/main.cf`. Дополнительные сервисы `/etc/postfix/master.cf`. Очередь сообщений `/var/spool/postfix/`.
``` bash
postconf [параметр] # просмотр параметров текущей конфигурации
```
`mailq` или `postqueue -p` просмотр очереди сообщений. Отправка писем производится через `mail` и `sendmail` 

> [!info] Postfix является MTA  (Mail Transfer Agent)
> Он отвечает только за доставку почты. Для чтения почты пользователями обычно дополнительно используют POP3/IMAP серверы.