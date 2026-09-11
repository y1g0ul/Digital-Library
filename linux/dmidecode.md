---
created-dt: 2026-09-11 10:20
tags:
  - review
---
Команда в [[Linux]] для чтения аппаратной информации из таблиц DMI/SMBIOS, которые предоставляет [[BIOS и UEFI]].

Позволяет узнать модель компьютера, материнскую плату, BIOS, RAM, сокеты CPU и другую информацию без разборки устройства.

```bash
sudo dmidecode [ключи]
```

|Ключ|Что делает|
|---|---|
|`-t`|показать информацию определённого типа|
|`-s`|вывести конкретное строковое значение|
|`-q`|более краткий вывод|
|`--no-sysfs`|не использовать данные из sysfs|

Часто используемые типы:

|Тип|Что показывает|
|---|---|
|`0`|BIOS|
|`1`|System Information|
|`2`|материнская плата|
|`3`|корпус|
|`4`|процессор|
|`16`|массив памяти|
|`17`|отдельные модули RAM|

Примеры:

```bash
sudo dmidecode
# показать всю доступную DMI/SMBIOS информацию

sudo dmidecode -t system
# информация о компьютере

sudo dmidecode -t baseboard
# информация о материнской плате

sudo dmidecode -t bios
# информация о BIOS/UEFI

sudo dmidecode -t processor
# информация о процессоре

sudo dmidecode -t memory
# информация об оперативной памяти

sudo dmidecode -t 17
# показать отдельные модули RAM
```

Полезные строковые запросы:

```bash
sudo dmidecode -s system-manufacturer
# производитель компьютера

sudo dmidecode -s system-product-name
# модель компьютера

sudo dmidecode -s baseboard-product-name
# модель материнской платы

sudo dmidecode -s bios-version
# версия BIOS
```

Пример для RAM:

```text
Memory Device
    Size: 16 GB
    Type: DDR4
    Speed: 3200 MT/s
    Manufacturer: Kingston
```

Важно:

`dmidecode` показывает данные, которые записаны производителем в SMBIOS. Они могут быть неполными или неточными.

Главная идея:

```text
BIOS / UEFI
    ↓
SMBIOS / DMI tables
    ↓
dmidecode
    ↓
информация о железе
```