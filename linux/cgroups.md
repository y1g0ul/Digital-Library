---
created-dt: 2026-08-20 13:10
tags:
  - review
sr-due: 2026-09-14
sr-interval: 3
sr-ease: 170
---
Механизм ядра [[Linux]] для объединения процессов в группы и управления тем, сколько системных ресурсов они могут использовать.

Через cgroups можно контролировать:
- CPU
- RAM
- количество [[процесс]]сов
- I/O диска
- некоторые другие ресурсы

Главная идея:
```text
Процессы
   ↓
cgroup
   ↓
лимиты и учёт ресурсов
```

Например:
```text
cgroup: web-app

CPU: максимум 2 ядра
RAM: максимум 1 GB
Processes: максимум 100
```

Все процессы внутри этой группы будут подчиняться этим ограничениям.

---

## Как устроено

cgroups образуют иерархию:
```text
/sys/fs/cgroup/
├── system.slice/
│   ├── ssh.service/
│   └── nginx.service/
│
└── user.slice/
    └── user-1000.slice/
```

Каждая cgroup может содержать:
- процессы;
- дочерние cgroups;
- настройки ограничений;
- статистику использования ресурсов.

Посмотреть:
```bash
ls /sys/fs/cgroup
# посмотреть корень cgroupfs

systemd-cgls
# показать дерево cgroups

systemd-cgtop
# показать использование ресурсов группами
```

---

## cgroup v2

Современные Linux обычно используют **cgroup v2** - единую иерархию для всех типов ресурсов.

Проверить:
```bash
stat -fc %T /sys/fs/cgroup
# cgroup2fs означает cgroup v2
```

Или:
```bash
mount | grep cgroup
# посмотреть смонтированную cgroupfs
```

---

## Как процесс попадает в cgroup

Каждый процесс принадлежит какой-либо cgroup.

Посмотреть для текущего shell:
```bash
cat /proc/self/cgroup
# показать cgroup текущего процесса
```

Для другого процесса:
```bash
cat /proc/<PID>/cgroup
# показать cgroup процесса
```

---

## Ограничение памяти

В cgroup v2 лимит памяти задаётся через:

```text
memory.max
```

Например:
```bash
cat /sys/fs/cgroup/<group>/memory.max
# посмотреть лимит памяти

cat /sys/fs/cgroup/<group>/memory.current
# текущее использование памяти
```

Если процесс превысит лимит и память нельзя освободить, может сработать [[OOM Killer]] внутри этой cgroup.
```text
Host RAM: 32 GB

cgroup limit: 512 MB
        ↓
процесс использовал 512 MB
        ↓
cgroup OOM
```

То есть на сервере свободная память ещё может быть.

---

## Ограничение CPU

В cgroup v2 используется:
```text
cpu.max
```

Например:
```text
50000 100000
```

означает, что группа может использовать CPU 50% времени за указанный период.

---

## Ограничение количества процессов

```text
pids.max
```

Например:
```bash
cat /sys/fs/cgroup/<group>/pids.max
# максимальное количество процессов

cat /sys/fs/cgroup/<group>/pids.current
# текущее количество процессов
```

Это защищает систему, например, от fork bomb.

---

## systemd и cgroups

[[systemd]] активно использует cgroups и автоматически помещает сервисы в отдельные группы.

Например:
```text
nginx.service
      ↓
cgroup
      ↓
nginx master
      ├── worker
      ├── worker
      └── worker
```

Посмотреть:
```bash
systemctl status nginx
# увидеть cgroup сервиса

systemd-cgls
# дерево cgroups
```

Для сервиса можно задавать ограничения:
```ini
MemoryMax=1G
CPUQuota=50%
TasksMax=100
```

---

## Docker и cgroups

[[Docker]] использует cgroups для ограничения ресурсов контейнеров.

Например:
```bash
docker run --memory=512m --cpus=1 nginx
```

Docker создаёт cgroup и задаёт:
```text
RAM ≤ 512 MB
CPU ≤ 1 CPU
```

Контейнер сам по себе не "имеет отдельную RAM". Это обычные Linux-процессы, которым ядро ограничивает ресурсы через cgroups.

---

## cgroups и namespaces

Эти механизмы часто используются вместе, но решают разные задачи.

|Механизм|Что делает|
|---|---|
|`namespaces`|изолируют процессы и дают им отдельное представление системы|
|`cgroups`|ограничивают и учитывают используемые ресурсы|

Упрощённо:
```text
Namespaces
→ что процесс видит

cgroups
→ сколько процесс может использовать
```

Именно сочетание этих механизмов лежит в основе контейнеров.

---

## Главное для собеседования

**cgroups - механизм ядра Linux для группировки процессов, ограничения и учёта их ресурсов.**

```text
Процессы
   ↓
cgroup
   ├── CPU limit
   ├── RAM limit
   ├── PID limit
   └── I/O limit
```

Их активно используют:

```text
systemd
Docker
Kubernetes
контейнеры
```

Главная связь:

```text
Container
   ↓
Namespaces → изоляция
cgroups    → ограничения ресурсов
```