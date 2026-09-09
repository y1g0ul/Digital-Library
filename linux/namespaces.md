---
created-dt: 2026-09-07 09:28
tags:
  - review
sr-due: 2026-09-10
sr-interval: 1
sr-ease: 224
---
Namespaces - механизм ядра [[Linux]], который позволяет **изолировать процессы друг от друга**.

Проще говоря, namespace создаёт для процесса отдельный «взгляд» на часть системы.

Например, два процесса могут видеть:
- разные процессы;
- разные сетевые интерфейсы;
- разные hostname;
- разные точки монтирования;
- разных пользователей.

При этом физически они работают на одном и том же ядре Linux.

```text
Один Linux kernel
      ↓
┌───────────────┐
│ Namespace A   │
│ process 1     │
│ eth0          │
│ hostname A    │
└───────────────┘

┌───────────────┐
│ Namespace B   │
│ process 1     │
│ eth0          │
│ hostname B    │
└───────────────┘
```

Именно namespaces являются одной из основных технологий, на которых построены [[container|контейнеры]].

## Зачем нужны namespaces

Без namespaces все процессы системы видели бы одно и то же окружение.

Например:
```text
Process A
Process B
Process C
```

Все они видят общие:
- [[PID]];
- сеть;
- mount points;
- hostname;
- IPC;
- пользователей.

Namespaces позволяют разделить это:
```text
Process A
  ↓
PID namespace
Network namespace
Mount namespace
```

и процесс начинает видеть собственную изолированную среду.

## Основные namespace

### PID namespace

Изолирует список процессов и PID.

Процесс внутри namespace может иметь:
```text
PID 1
```

хотя снаружи у него будет совершенно другой PID.

Например:
```text
Host:
PID 5421 → nginx

Container:
PID 1 → nginx
```

То есть один и тот же процесс имеет разные PID в зависимости от того, из какого namespace на него смотреть.

Это особенно важно для контейнеров.

---

### Network namespace

Изолирует сетевой стек.

Каждый network namespace может иметь собственные:
- интерфейсы;
- [[networks/IP|IP]]-адреса;
- маршруты;
- [[ARP]]/[[NDP]];
- firewall rules;
- loopback.

```text
Namespace A:
eth0 → 10.0.0.2

Namespace B:
eth0 → 172.16.0.2
```

Оба работают на одном Linux-хосте, но видят разные сети.

---

### Mount namespace

Изолирует точки монтирования файловых систем.

Например:
```text
Host:
/
├── home
├── var
└── mnt
```

А внутри namespace:
```text
/
├── app
├── tmp
└── proc
```

Процесс внутри namespace может видеть совершенно другую структуру mount points.

Именно поэтому контейнер может иметь свою файловую систему, хотя ядро у него общее с хостом.

---

### UTS namespace

Изолирует:
- hostname;
- domain name.

Например:

```text
Host:
hostname = server01

Container:
hostname = web01
```

---

### IPC namespace

Изолирует механизмы межпроцессного взаимодействия:
- shared memory;
- message queues;
- semaphores.

Процессы из разных IPC namespace не видят IPC-ресурсы друг друга.

---

### User namespace

Изолирует пользователей и UID/GID.

Например:
```text
В контейнере:
UID 0 → root

На хосте:
UID 100000
```

То есть процесс может считать себя `root` внутри namespace, но на хосте у него нет настоящих root-прав.

Это важный механизм безопасности.

---

### Cgroup namespace

Изолирует представление [[cgroups]].

Процесс внутри namespace может видеть только свою часть иерархии cgroup, а не всю структуру хоста.

---

### Time namespace

Позволяет изолировать некоторые системные часы.

Например, процессы в разных namespace могут видеть разное значение времени относительно boot time.

Используется реже, чем PID или network namespace.

## Основные типы

|Namespace|Что изолирует|
|---|---|
|`pid`|процессы и PID|
|`net`|сеть|
|`mnt`|mount points|
|`uts`|hostname|
|`ipc`|IPC|
|`user`|UID/GID|
|`cgroup`|представление cgroups|
|`time`|некоторые системные часы|

## Namespace и контейнеры

Контейнер - это не отдельная виртуальная машина.

Он использует ядро хоста, но благодаря namespaces получает изолированное окружение.

```text
Linux kernel
      ↓
┌───────────────────┐
│ Container A       │
│ PID namespace     │
│ NET namespace     │
│ MNT namespace     │
│ UTS namespace     │
└───────────────────┘

┌───────────────────┐
│ Container B       │
│ PID namespace     │
│ NET namespace     │
│ MNT namespace     │
│ UTS namespace     │
└───────────────────┘
```

Docker, Podman и другие container runtime используют namespaces вместе с [[cgroups]].

Очень грубо:
```text
Namespaces → что процесс может видеть

cgroups → сколько ресурсов процесс может использовать
```

Например:
```text
Container
   ↓
namespaces
   ↓
видит только свои процессы и сеть

cgroups
   ↓
может использовать максимум 2 GB RAM и 2 CPU
```

## Просмотр namespaces процесса

Namespaces процесса можно увидеть через `/proc`:

```bash
ls -l /proc/<PID>/ns/
```

Например:

```text
cgroup
ipc
mnt
net
pid
pid_for_children
time
user
uts
```

Каждая запись указывает на namespace, в котором находится процесс.

Можно посмотреть свой процесс:

```bash
ls -l /proc/$$/ns/
```

## Полезные утилиты

Для работы с namespaces используются [[unshare]] создаёт новый namespace и [[nsenter]] позволяет войти в namespace другого процесса.

Например, можно создать shell в новом PID namespace или войти в network namespace контейнера.

## Важная идея

Namespace не создаёт отдельный Linux kernel.

```text
Container A ─┐
Container B ─┼─> один Linux kernel
Container C ─┘
```

Он только меняет то, **какую часть системы видит процесс**.

Поэтому namespaces дают очень лёгкую изоляцию по сравнению с виртуальными машинами.

## Кратко

```text
Namespace = изолированный взгляд процесса на систему
```

Основные:
```text
pid    → процессы
net    → сеть
mnt    → файловые системы
uts    → hostname
ipc    → IPC
user   → пользователи
cgroup → cgroups
time   → время
```

Главная идея:
```text
Namespaces → изоляция
Cgroups    → ограничения ресурсов
```

Вместе они являются основой Linux-контейнеров.