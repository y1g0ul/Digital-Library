---
created-dt: 2025-11-17 12:03
tags:
  - review
sr-due: 2026-09-01
sr-interval: 125
sr-ease: 190
---
В [[git]] это просто ссылки на определенный [[commit]].

`git branch [name]` - создать ветку
- `-f` - передвинуть [[branch]] на другой [[commit]] (`git branch -f main HEAD~2` - Передвинуть main на 2 [[commit]]а назад от [[git/head|head]])
- `-u` - указать [[branch]] и отслеживать удаленную [[branch]] (`git branch -u o/main foo`, а если находиться на ветке `foo`, то её можно не указывать - `git branch -u o/main`)

`git checkout [name]` - переключиться на новую ветку или [[commit]]. (переместить [[linux/head|head]])
- ` -b [yourbranchname]` - создать новую ветку и сразу на нее переключиться. Так же можно создать [[branch]] которая будет отслеживать [[branch]] из удаленного репозитория - `git checkout -b totallyNotMain origin/main` . 

