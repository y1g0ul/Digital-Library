---
created-dt: 2025-11-25 11:47
tags:
  - review
sr-due: 2026-08-27
sr-interval: 139
sr-ease: 210
---
Команда в [[git]] которая по сути заменяет собой 2 другие команды [[fetch]] и [[merge]]. Она вытягивет изменения из удаленного репозитория и обьядиняет с нашим. 
``` bash
git pull <опция> <remote> <ветка>
```

![[git pull]]

|Опция|Назначение|
|---|---|
|`--rebase`|Получить изменения и выполнить rebase вместо merge|
|`--no-rebase`|Использовать merge вместо rebase|
|`--ff-only`|Разрешить только fast-forward обновление|
|`--all`|Получить изменения со всех remote|
Так же можно добавтить аргументы `<источник>:<получатель>`. 
``` bash
git pull # Получить изменения из origin текущей ветки
git pull origin main # Получить ветку main из origin
git pull --rebase # Получить изменения через rebase
git pull --ff-only # Обновить только при fast-forward
```