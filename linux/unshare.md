---
created-dt: 2026-09-07 09:55
tags:
  - review
sr-due: 2026-09-17
sr-interval: 7
sr-ease: 250
---
Команда в [[Linux]] для создания новых [[namespaces]] и запуска процесса внутри них. Используется для изоляции процессов и демонстрации того, как работают контейнеры.

```bash
unshare [ключи] [команда]
```

|Ключ|Что делает|
|---|---|
|`-m`|создать новый mount namespace|
|`-n`|создать новый network namespace|
|`-p`|создать новый PID namespace|
|`-u`|создать новый UTS namespace|
|`-i`|создать новый IPC namespace|
|`-U`|создать новый user namespace|
|`-C`|создать новый cgroup namespace|
|`-f`|fork перед запуском команды|
|`--mount-proc`|смонтировать отдельный `/proc`|

Примеры:
```bash
unshare -n bash
# создать shell с отдельным network namespace

unshare -u bash
# создать отдельный UTS namespace

unshare -m bash
# создать отдельный mount namespace

unshare -pf --mount-proc bash
# создать отдельный PID namespace с собственным /proc

unshare -Ur bash
# создать user namespace и получить root внутри него
```

Пример с сетью:
```bash
unshare -n bash
ip a
# внутри будет отдельное сетевое пространство
```

Пример с PID:
```bash
unshare -pf --mount-proc bash
ps aux
# процессы будут видны относительно нового PID namespace
```

Главная идея:
```text
unshare
→ создаёт новый namespace
→ запускает процесс внутри него

nsenter
→ входит в уже существующий namespace
```

Связь с контейнерами:
```text
Namespaces
→ изоляция

unshare
→ вручную создать изоляцию

nsenter
→ войти внутрь этой изоляции
```