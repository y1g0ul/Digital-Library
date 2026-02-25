---
created-dt: 2026-01-13 08:05
tags:
  - review
sr-due: 2026-04-23
sr-interval: 57
sr-ease: 250
---
Команда для просмотра скачанных [[image]] в [[docker]].
``` shell
y1g0ul@DESKTOP-195S4U1:~$ docker images
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
hello-world:latest   d4aaab6242e0       25.9kB         9.52kB
```
- `-a` — показать **все образы**, включая промежуточные слои
- `-q` — показать только **ID образов**
- `-f` — показать образы по фильтру

| Фильтр       | Пример                                 | Что делает                                                                               |
| ------------ | -------------------------------------- | ---------------------------------------------------------------------------------------- |
| `dangling=`  | `docker images -f dangling=true`       | Показать **"висящие" образы**, которые **не имеют тегов** и не используются контейнерами |
| `label=`     | `docker images -f label=env=prod`      | Показать образы с определённой меткой (label)                                            |
| `before=`    | `docker images -f before=nginx:latest` | Показать образы, созданные **до указанного образа**                                      |
| `since=`     | `docker images -f since=ubuntu:20.04`  | Показать образы, созданные **после указанного образа**                                   |
| `reference=` | `docker images -f reference="myapp:*"` | Показать образы по шаблону имени и тегу                                                  |