---
created-dt: 2026-09-04 10:59
tags:
  - review
sr-due: 2026-09-12
sr-interval: 1
sr-ease: 200
---
**Linux kernel** - ядро операционной системы [[Linux]]. Оно является главным слоем между пользовательскими программами и аппаратным обеспечением компьютера.

```text
Applications
bash, nginx, postgres, docker...
        ↓
   system calls
        ↓
   Linux kernel
        ↓
CPU / RAM / disks / network / devices
```

Программы обычно не работают с железом напрямую. Они обращаются к ядру, а ядро уже выполняет необходимые операции.

Например:
```text
cat file.txt
    ↓
read()
    ↓
Linux kernel
    ↓
filesystem
    ↓
driver
    ↓
disk
```

## User space и kernel space

Linux разделяет систему на два основных пространства:
```text
User space
Kernel space
```

### User space

Здесь работают обычные [[процесс]]ы:
```text
bash
nginx
postgres
python
systemd
docker
```

У них ограниченные права и нет прямого доступа ко всем ресурсам системы.

### Kernel space

Здесь работает само ядро и его компоненты.

```text
          User space
-------------------------------
bash
nginx
postgres
python
-------------------------------
         system calls
-------------------------------
         Kernel space
-------------------------------
scheduler
memory manager
VFS
network stack
drivers
namespaces
cgroups
-------------------------------
           Hardware
```

Ядро имеет привилегированный доступ к CPU, памяти и устройствам.

## System calls

**System call** - способ, которым программа просит ядро выполнить системную операцию.

Например:
```text
open()
read()
write()
close()
socket()
fork()
exec()
```

Обычные вычисления могут выполняться полностью в user space:
```text
2 + 2
```

Но для работы с системными ресурсами требуется ядро:
```text
файл
сеть
процесс
память
устройство
        ↓
system call
        ↓
kernel
```

## Управление процессами

Ядро управляет процессами и потоками.

Оно отвечает за:
- создание и завершение процессов;
- их состояния;
- распределение CPU;
- переключение между процессами.

Этим занимается [[scheduler]].

```text
CPU
 ↓
nginx
 ↓
postgres
 ↓
bash
 ↓
kworker
```

Если CPU один, одновременно реально выполняется один поток, а ядро быстро переключает их. Если CPU несколько, несколько потоков могут выполняться параллельно.

## Управление памятью

Ядро управляет оперативной памятью и виртуальной памятью.

Каждый процесс получает своё виртуальное адресное пространство:
```text
Process A
0x0000 ...
      ↓
virtual memory
      ↓
RAM
```

И:
```text
Process B
0x0000 ...
      ↓
virtual memory
      ↓
другая область RAM
```

Поэтому процессы обычно не могут напрямую читать или изменять память друг друга.

Ядро отвечает за:
- выделение памяти;
- освобождение памяти;
- virtual memory;
- page cache;
- swap;
- защиту памяти.

## Файловые системы

Ядро предоставляет программам единый интерфейс работы с файлами.

Для этого используется **VFS - Virtual File System**.

```text
Application
     ↓
    VFS
     ↓
┌────┬─────┬──────┐
ext4 xfs  btrfs
```

Программа может использовать:
```text
open()
read()
write()
```

не думая о том, какая файловая система находится под ними.

## Драйверы

**Driver** - компонент, который позволяет ядру работать с конкретным устройством.

Например:
```text
Application
    ↓
Kernel
    ↓
NVMe driver
    ↓
SSD
```

или:
```text
Kernel
   ↓
Network driver
   ↓
Network adapter
```

Именно драйвер знает, как взаимодействовать с конкретным оборудованием.

## Сеть

Сетевой стек Linux в значительной степени работает внутри ядра.

```text
curl
 ↓
socket()
 ↓
kernel
 ↓
TCP
 ↓
IP
 ↓
routing
 ↓
network driver
 ↓
NIC
```

В ядре реализованы или управляются:
- [[TCP]][[UDP]];
- [[networks/IP|IP]];
- routing;
- network [[namespaces]]s;
- firewall;
- [[socket]]s;
- сетевые интерфейсы.

Например, [[nftables]] настраивает правила, которые затем применяет сетевой стек ядра.

## Прерывания

Устройства могут сообщать CPU о событиях через **hardware interrupts**.

Например:
```text
Network card
     ↓
    IRQ
     ↓
    CPU
     ↓
Linux kernel
```

Это может происходить, когда:
- пришёл сетевой пакет;
- завершилась операция диска;
- нажата клавиша;
- устройство требует внимания.

Ядро старается быстро выполнить срочную часть обработки.

Менее срочная работа может быть отложена:
```text
IRQ
 ↓
быстрая обработка
 ↓
softirq / workqueue
 ↓
ksoftirqd / kworker
```

## Kernel threads

У ядра существуют собственные [[thread|потоки]]:
```text
[kworker/...]
[ksoftirqd/...]
[kthreadd]
[rcu...]
```

Они называются **kernel threads**.

Например [[kworker]] выполняет отложенные задачи ядра из `workqueue`.

## Namespaces

[[namespaces]] - механизм ядра для изоляции процессов.

Они позволяют процессам иметь отдельное представление:
- [[PID]];
- сети;
- mount points;
- hostname;
- пользователей;
- IPC.

```text
Container A
   ↓
PID namespace
NET namespace
MNT namespace
```

При этом используется то же самое Linux kernel.

## Cgroups

[[cgroups]] позволяют ядру контролировать использование ресурсов процессами.

Например:
```text
Container
   ↓
max 2 GB RAM
max 2 CPU
```

То есть:
```text
namespaces → что процесс видит

cgroups → сколько ресурсов процесс может использовать
```

Эти механизмы являются важной частью работы Linux-контейнеров.

## Ядро и Docker

Docker не создаёт отдельное ядро для каждого контейнера.

Он использует возможности существующего Linux kernel:
```text
Docker
   ↓
Linux kernel
   ↓
namespaces
cgroups
networking
mounts
```

Поэтому:
```text
Container A ─┐
Container B ─┼─> Linux kernel
Container C ─┘
```

Контейнеры изолированы, но работают на одном ядре хоста.

## Загрузка ядра

При запуске системы происходит примерно такая цепочка:
```text
BIOS / UEFI
     ↓
bootloader
     ↓
Linux kernel
     ↓
initramfs
     ↓
systemd
     ↓
user space
```

Файл ядра обычно находится в:
```text
/boot/vmlinuz-...
```

При загрузке он помещается в RAM и работает там всё время жизни системы.

Версию текущего ядра можно посмотреть:
```bash
uname -r
```

## Kernel panic

Если обычный процесс падает, ядро продолжает работать:
```text
nginx crash
```

Но если происходит критическая ошибка самого ядра, система обычно уже не может нормально продолжать работу:
```text
kernel panic
```

Это происходит потому, что ядро управляет всей системой.

## Общая схема работы

```text
          Applications
      bash / postgres / nginx
               ↓
          system calls
               ↓
        Linux kernel
               ↓
 ┌─────────────┼──────────────┐
 ↓             ↓              ↓
Processes     Memory        Filesystems
Scheduler     VM            VFS
 ↓             ↓              ↓
          Network stack
               ↓
            Drivers
               ↓
            Hardware
```

## Кратко

Linux kernel отвечает за:
```text
CPU       → scheduler
RAM       → memory management
Files     → VFS / filesystems
Devices   → drivers
Network   → TCP/IP stack
Security  → permissions / isolation
Containers→ namespaces + cgroups
Events    → IRQ / softirq / workqueue
```

Главная идея:
> **Linux kernel - привилегированный управляющий слой, который контролирует системные ресурсы и предоставляет доступ к ним пользовательским программам через system calls.**