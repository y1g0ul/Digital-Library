---
created-dt: 2026-06-01 11:31
tags:
  - review
sr-due: 2026-06-22
sr-interval: 3
sr-ease: 210
---
Команда в [[Linux]] для получения записей из системных баз данных (NSS)
``` bash
getent база_данных [ключ]
```

|Ключ|Что делает|
|---|---|
|`passwd`|информация о пользователях|
|`group`|информация о группах|
|`shadow`|информация о паролях (требуются права root)|
|`hosts`|информация о хостах|
|`services`|сетевые сервисы и порты|
|`protocols`|сетевые протоколы|
|`networks`|сетевые сети|
|`ethers`|MAC-адреса|
|`aliases`|почтовые алиасы|
``` bash
getent passwd                 # все пользователи

getent passwd nikita          # информация о пользователе

getent group                  # все группы

getent group sudo             # информация о группе

getent hosts google.com       # разрешение имени через NSS

getent hosts 8.8.8.8          # поиск по IP

getent services ssh           # информация о сервисе ssh

getent protocols tcp          # информация о протоколе TCP

getent shadow root            # запись root из /etc/shadow
```

> [!info] Не читает напрямую файлы
> Команда использует NSS (Name Service Switch), поэтому может получать данные не только из локальных файлов, но и из LDAP, NIS, Active Directory и других источников.


