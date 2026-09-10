---
created-dt: 2026-09-07 09:55
tags:
  - review
sr-due: 2026-09-18
sr-interval: 8
sr-ease: 250
---
Команда в [[Linux]] для входа в [[namespaces]] другого процесса. Позволяет посмотреть систему «глазами» другого процесса, например процесса внутри контейнера.

```bash
nsenter [ключи] -t <PID> [команда]
```

|Ключ|Что делает|
|---|---|
|`-t <PID>`|выбрать процесс, чьи namespace использовать|
|`-m`|войти в mount namespace|
|`-n`|войти в network namespace|
|`-p`|войти в PID namespace|
|`-u`|войти в UTS namespace|
|`-i`|войти в IPC namespace|
|`-C`|войти в cgroup namespace|
|`-U`|войти в user namespace|
|`-a`|войти во все доступные namespace процесса|

Примеры:
```bash
nsenter -t 1234 -n ip a
# посмотреть сеть глазами процесса 1234

nsenter -t 1234 -m ls /
# посмотреть файловую систему его mount namespace

nsenter -t 1234 -p ps aux
# выполнить ps внутри PID namespace

nsenter -t 1234 -a /bin/bash
# войти во все namespace процесса и открыть shell
```

Полезно для контейнеров:
```bash
docker inspect -f '{{.State.Pid}}' container_name
# узнать PID контейнера на хосте

nsenter -t <PID> -a /bin/bash
# войти в namespace контейнера
```

Главная идея:
```text
nsenter
→ не создаёт namespace
→ подключает процесс к уже существующим namespace
```