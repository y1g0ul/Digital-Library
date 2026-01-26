---
created-dt: 2026-01-13 09:15
tags:
  - review
sr-due: 2026-02-19
sr-interval: 24
sr-ease: 250
---
Команда для удаления [[image]] в [[docker]].
``` shell
docker rmi [OPTIONS] IMAGE:TAG [IMAGE...]
```

Если мы явно не укажем тег то будет удален [[image]] с тегом `latest`. Так же мы не можем удалить образ с которым связан [[container]].