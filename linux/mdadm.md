---
created-dt: 2026-09-02 10:35
tags:
  - review
sr-due: 2026-09-15
sr-interval: 4
sr-ease: 210
---
Утилита в [[Linux]] для создания, настройки, мониторинга и восстановления программных [[RAID]]-массивов.

Обычно работает с устройствами вида:
```text
/dev/md0
/dev/md1
/dev/md2
```

Общая схема:
```text
/dev/sdb ─┐
          ├─> mdadm ─> /dev/md0
/dev/sdc ─┘
```

После создания массива `/dev/md0` используется как обычное блочное устройство:
```text
/dev/md0
   ↓
filesystem / LVM
   ↓
mount
```

## Установка

Debian / Ubuntu:
```bash
sudo apt install mdadm
```

Fedora:
```bash
sudo dnf install mdadm
```

## Создание RAID

Общий синтаксис:
```bash
sudo mdadm --create /dev/mdX \
  --level=<RAID> \
  --raid-devices=<количество> \
  <диски>
```

### RAID 1

Например, создать RAID 1 из двух дисков:
```bash
sudo mdadm --create /dev/md0 \
  --level=1 \
  --raid-devices=2 \
  /dev/sdb /dev/sdc
```

Получится:
```text
/dev/sdb ─┐
          ├─ RAID 1 ─> /dev/md0
/dev/sdc ─┘
```

### RAID 5

```bash
sudo mdadm --create /dev/md0 \
  --level=5 \
  --raid-devices=3 \
  /dev/sdb /dev/sdc /dev/sdd
```

### RAID 6

```bash
sudo mdadm --create /dev/md0 \
  --level=6 \
  --raid-devices=4 \
  /dev/sdb /dev/sdc /dev/sdd /dev/sde
```

### RAID 10

```bash
sudo mdadm --create /dev/md0 \
  --level=10 \
  --raid-devices=4 \
  /dev/sdb /dev/sdc /dev/sdd /dev/sde
```

## Просмотр состояния

Один из самых полезных способов:
```bash
cat /proc/mdstat
```

Пример:
```text
md0 : active raid1 sdc[1] sdb[0]
      976630336 blocks super 1.2 [2/2] [UU]
```

Главное:
```text
[2/2]
```

означает:
```text
2 ожидаемых диска / 2 рабочих диска
```

А:
```text
[UU]
```

показывает состояние каждого диска.
```text
U  → диск работает
_  → диск отсутствует или неисправен
```

Примеры:
```text
[UU] → оба диска работают
[U_] → второй диск отсутствует
[_U] → первый диск отсутствует
```

## Подробная информация о массиве

```bash
sudo mdadm --detail /dev/md0
```

Пример информации:
```text
Raid Level : raid1
Array Size : ...
Raid Devices : 2
Active Devices : 2
Working Devices : 2
Failed Devices : 0
State : clean
```

Это одна из основных команд при диагностике RAID.

## Информация о диске

Можно посмотреть RAID-метаданные непосредственно на диске:
```bash
sudo mdadm --examine /dev/sdb
```

Например можно увидеть:
- UUID массива;
- RAID level;
- номер диска;
- количество устройств;
- состояние metadata.

Это полезно, если нужно понять, к какому массиву принадлежал диск.

## Добавление диска

Добавить новый диск в существующий массив:
```bash
sudo mdadm --add /dev/md0 /dev/sdd
```

Например после замены неисправного диска:
```text
/dev/sdb ─┐
          ├─ RAID 1 ─> /dev/md0
/dev/sdd ─┘
```

После добавления обычно начинается rebuild.

Проверить:
```bash
cat /proc/mdstat
```

Можно увидеть:
```text
recovery = 42.5%
```

## Удаление неисправного диска

Обычно сначала диск помечается как неисправный:
```bash
sudo mdadm --fail /dev/md0 /dev/sdc
```

Затем удаляется из массива:
```bash
sudo mdadm --remove /dev/md0 /dev/sdc
```

Получается последовательность:
```text
disk
 ↓
--fail
 ↓
failed
 ↓
--remove
 ↓
удалён из массива
```

После этого можно заменить диск и добавить новый:
```bash
sudo mdadm --add /dev/md0 /dev/sdd
```

## Замена диска

Типичный сценарий:
```bash
sudo mdadm --fail /dev/md0 /dev/sdc
sudo mdadm --remove /dev/md0 /dev/sdc
```

Физически меняем диск.

Затем:
```bash
sudo mdadm --add /dev/md0 /dev/sdd
```

После чего проверяем восстановление:
```bash
watch cat /proc/mdstat
```

## Остановка массива
```bash
sudo mdadm --stop /dev/md0
```

После этого устройство `/dev/md0` перестаёт быть активным.

Перед остановкой файловая система должна быть размонтирована:
```bash
sudo umount /mnt/storage
sudo mdadm --stop /dev/md0
```

## Запуск существующего массива

Если RAID уже существует на дисках:
```bash
sudo mdadm --assemble /dev/md0 /dev/sdb /dev/sdc
```

Или автоматически найти массивы:
```bash
sudo mdadm --assemble --scan
```

`--assemble` отличается от `--create`:

```text
--create
создать новый RAID

--assemble
собрать уже существующий RAID
```

## Удаление RAID-метаданных

После удаления массива на дисках могут оставаться metadata RAID.

Их можно удалить:

```bash
sudo mdadm --zero-superblock /dev/sdb
sudo mdadm --zero-superblock /dev/sdc
```

После этого диски больше не будут определяться как участники старого массива.

Важно использовать эту команду осторожно.

## Создание файловой системы

После создания RAID:
```text
/dev/sdb ─┐
          ├─ RAID ─> /dev/md0
/dev/sdc ─┘
```

можно создать файловую систему:
```bash
sudo mkfs.ext4 /dev/md0
```

Затем:
```bash
sudo mkdir /mnt/raid
sudo mount /dev/md0 /mnt/raid
```

Проверить:
```bash
df -h
```

## Конфигурация mdadm

Информация о массивах может храниться в конфигурации `mdadm`.

Часто используется файл:
```text
/etc/mdadm/mdadm.conf
```

или в некоторых дистрибутивах:
```text
/etc/mdadm.conf
```

Посмотреть информацию, которую можно добавить в конфигурацию:
```bash
sudo mdadm --detail --scan
```

Пример:
```text
ARRAY /dev/md0 metadata=1.2 UUID=...
```

Это помогает системе автоматически находить и собирать массив при загрузке.

## Полезные команды

|Команда|Что делает|
|---|---|
|`mdadm --create`|создать новый RAID|
|`mdadm --assemble`|собрать существующий RAID|
|`mdadm --detail`|показать состояние массива|
|`mdadm --examine`|посмотреть RAID metadata на диске|
|`mdadm --add`|добавить диск|
|`mdadm --fail`|пометить диск как неисправный|
|`mdadm --remove`|удалить диск из массива|
|`mdadm --stop`|остановить массив|
|`mdadm --zero-superblock`|удалить RAID metadata с диска|
|`mdadm --detail --scan`|найти существующие массивы|

## Полезный сценарий диагностики

Если есть подозрение на проблему с RAID:
```bash
cat /proc/mdstat
```

Затем:
```bash
sudo mdadm --detail /dev/md0
```

Если проблема с конкретным диском:
```bash
sudo mdadm --examine /dev/sdb
```

Получается:
```text
/proc/mdstat
     ↓
общее состояние
     ↓
mdadm --detail
     ↓
подробности массива
     ↓
mdadm --examine
     ↓
metadata конкретного диска
```

## Кратко

```text
Создать:
mdadm --create

Посмотреть:
cat /proc/mdstat
mdadm --detail

Добавить диск:
mdadm --add

Пометить неисправным:
mdadm --fail

Удалить:
mdadm --remove

Собрать существующий RAID:
mdadm --assemble

Остановить:
mdadm --stop
```

`mdadm` - основная утилита управления программным [[RAID]] в Linux. Она позволяет создать массив, следить за его состоянием, заменять неисправные диски и восстанавливать массив после отказов.