---
created-dt: 2025-12-17 03:24
tags:
  - review
sr-due: 2026-08-25
sr-interval: 151
sr-ease: 250
---
Если у папки в [[Linux]] стоит `+t`, то удалить/переименовать файл внутри неё может только:  
  1. Владелец файла.  
  2. Владелец папки .  
  3. Пользователь с правами `root`.

Различия  sticky bit:
- `t` - sticky bit включён и есть `x` 
	`drwxrwxrwt 8 root root 4096 Dec 24 07:41 /tmp/`
- `T` - sticky bit включён, но `x` **запрещён**
	`drwx-wx--T 2 root crontab 4096 Mar 31  2024 crontabs`