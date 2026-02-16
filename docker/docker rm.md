---
created-dt: 2026-01-13 09:03
tags:
  - review
sr-due: 2026-04-11
sr-interval: 54
sr-ease: 250
---
Команда для удаления [[container]] в [[docker]].
``` shell
docker rm [OPTIONS] CONTAINER [CONTAINER...]
```
- `-f` - Принудительное удаление запущенного контейнера (использует [[сигнал]] SIGKILL)
- `-v` - Удалите анонимные [[docker volume]], связанные с [[container]].
