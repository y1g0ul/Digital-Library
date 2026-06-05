---
created-dt: 2026-06-01 11:14
tags:
  - review
sr-due: 2026-06-12
sr-interval: 7
sr-ease: 250
---
Команда в [[Linux]] для изменения параметров группы
``` bash
groupmod [опции] группа
```

|Ключ|Что делает|
|---|---|
|`-n`|изменить имя группы|
|`-g`|изменить GID группы|
|`-o`|разрешить использование неуникального GID|
``` bash
groupmod -n developers devs    # переименовать группу

groupmod -g 2000 developers    # изменить GID

groupmod -o -g 1000 testgroup  # использовать существующий GID
```

> [!warning] Изменение GID группы не меняет автоматически группу-владельца файлов на диске.  
> Для уже существующих файлов может потребоваться:  
>`find / -group OLD_GID -exec chgrp NEW_GROUP {} \;`

