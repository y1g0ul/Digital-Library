---
created-dt: 2026-07-14 15:28
tags:
  - review
sr-due: 2026-07-23
sr-interval: 3
sr-ease: 250
---
Команда для управления удалёнными репозиториями  в [[git]].
``` bash
git remote <опция> <имя> <URL>
```

|Опция|Назначение|
|---|---|
|`-v`|Показать URL удалённых репозиториев|
|`add`|Добавить удалённый репозиторий|
|`remove`|Удалить удалённый репозиторий|
|`rename`|Переименовать удалённый репозиторий|
|`set-url`|Изменить URL удалённого репозитория|
``` bash
git remote -v # Показать удалённые репозитории
git remote add origin https://github.com/user/repo.git # Добавить remote
git remote remove origin # Удалить remote
git remote rename origin upstream # Переименовать remote
git remote set-url origin https://github.com/user/new.git # Изменить URL
```

