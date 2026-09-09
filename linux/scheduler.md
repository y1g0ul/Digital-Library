---
created-dt: 2026-09-07 10:45
tags:
  - review
sr-due: 2026-09-10
sr-interval: 1
sr-ease: 218
---
Часть ядра [[Linux]], которая решает, **какой поток получит процессорное время и на каком CPU он будет выполняться**.

Упрощённо:
```text
Runnable tasks
     ↓
  scheduler
     ↓
    CPU
```

Если задач больше, чем доступных CPU, scheduler быстро переключает их между процессорами.

## Что планирует scheduler

Scheduler работает не столько с «программами», сколько с задачами/потоками.

Например:
```text
nginx
├── thread 1
├── thread 2
├── thread 3
└── thread 4
```

Каждый поток может планироваться отдельно.

То есть правильнее:
```text
threads
   ↓
scheduler
   ↓
CPU
```

## Зачем он нужен

Допустим, есть один CPU и несколько процессов:
```text
bash
nginx
postgres
python
```

В конкретный момент времени CPU выполняет один поток:
```text
CPU
 ↓
nginx
 ↓
postgres
 ↓
python
 ↓
bash
```

Scheduler быстро переключает их, из-за чего создаётся ощущение одновременной работы.

Если CPU несколько:
```text
CPU 0 → nginx
CPU 1 → postgres
CPU 2 → python
CPU 3 → kworker
```

scheduler распределяет задачи между ними.

## Runnable и sleeping

Scheduler в основном работает с задачами, которые готовы выполняться.

Такая задача находится в состоянии:
```text
runnable
```

Если процесс ждёт какое-то событие, например диск:
```text
postgres
   ↓
read()
   ↓
ждёт данные
```

ему пока не нужен CPU.

Он переходит в состояние ожидания:
```text
sleeping / waiting
```

Scheduler отдаёт CPU другой задаче.

Когда данные приходят:
```text
disk
 ↓
IRQ
 ↓
kernel
 ↓
postgres → runnable
 ↓
scheduler
```

процесс снова может получить CPU.

Главная идея:
> Scheduler не тратит процессорное время на задачи, которым сейчас нечего выполнять.

## Context switch

Переключение CPU между потоками называется **context switch**.

```text
nginx
  ↓
context switch
  ↓
postgres
```

Перед переключением ядро сохраняет состояние текущего потока:
- регистры CPU;
- instruction pointer;
- stack pointer;
- другую необходимую информацию.

После этого восстанавливается состояние следующего потока.

```text
nginx state
   ↓ save

postgres state
   ↑ restore
```

Context switch сам требует ресурсов, поэтому слишком большое количество переключений может снижать производительность.

## Run queue

У CPU есть задачи, которые готовы выполняться.

Упрощённо это можно представить как очередь:
```text
CPU 0:
nginx
bash
python

CPU 1:
postgres
kworker
```

Scheduler выбирает следующую runnable-задачу и запускает её.

## Балансировка между CPU

Scheduler старается распределять нагрузку между процессорами.

Плохо:
```text
CPU 0:
nginx
postgres
python
bash

CPU 1:
пусто
```

Scheduler может перенести часть задач:
```text
CPU 0        CPU 1
nginx        postgres
bash         python
```

Это называется **load balancing**.

## Priority и nice

Не все процессы имеют одинаковое влияние на планирование.

Для обычных процессов можно использовать [[nice]].

Диапазон:
```text
-20 ... 19
```

Где:
```text
-20 → выше приоритет
  0 → обычное значение
 19 → ниже приоритет
```

Например:
```bash
nice -n 10 command
```

Такой процесс будет более «уступчивым» по отношению к другим задачам.

Посмотреть или изменить значение работающего процесса можно через:
```bash
renice
```

`nice` не задаёт конкретный процент CPU, а влияет на то, как scheduler распределяет процессорное время между конкурирующими задачами.

## CPU affinity

Процессу можно ограничить набор CPU, на которых он имеет право выполняться.

Например:
```bash
taskset -c 0 command
```

Процесс сможет работать только на:
```text
CPU 0
```

Посмотреть affinity существующего процесса:
```bash
taskset -cp <PID>
```

Scheduler продолжит управлять процессом, но не сможет отправить его на CPU вне разрешённого набора.

## Scheduler и kernel threads

Scheduler планирует не только пользовательские процессы.

Kernel threads тоже участвуют в планировании:
```text
nginx
postgres
bash
kworker
ksoftirqd
```

Например [[kworker]] - обычная для scheduler задача, которой также нужно выделить CPU.

## Realtime scheduling

Linux поддерживает специальные realtime-политики планирования:
```text
SCHED_FIFO
SCHED_RR
```

Они используются для задач, где особенно важна минимальная задержка получения CPU.

Realtime-задачи могут иметь очень высокий приоритет, поэтому неправильная настройка способна мешать выполнению обычных процессов.

## Scheduler не понимает назначение программы

Scheduler не думает:
```text
PostgreSQL важнее Python
```

Он принимает решения на основе параметров задачи:
```text
state
priority
nice
runtime
CPU affinity
scheduling policy
load
```

То есть он работает по алгоритмам, а не по смыслу программы.

## Связь с load average

Если CPU занят, а много задач находятся в состоянии runnable:
```text
CPU → process A

ждут CPU:
process B
process C
process D
```

возникает очередь за процессорным временем.

Количество таких задач связано с общей нагрузкой системы и показателем [[load average]].

## Общая схема

```text
Processes / Threads
        ↓
   runnable tasks
        ↓
     scheduler
    ↙    ↓    ↘
 CPU0   CPU1   CPU2
```

Если задача начинает ждать ресурс:
```text
process
   ↓
wait for disk/network/timer
   ↓
sleeping
```

Она временно перестаёт конкурировать за CPU.

Когда происходит нужное событие:
```text
event
 ↓
kernel
 ↓
process → runnable
 ↓
scheduler
```

## Кратко

**Scheduler** отвечает за:
```text
выбор следующей runnable-задачи
        ↓
выделение ей CPU
        ↓
context switch
        ↓
балансировку задач между CPU
```

Главная идея:

> **Scheduler - механизм ядра Linux, который распределяет процессорное время между потоками и решает, какой поток, на каком CPU и когда будет выполняться.**