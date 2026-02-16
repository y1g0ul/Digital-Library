---
created-dt: 2026-01-19 10:55
tags:
  - review
sr-due: 2026-03-24
sr-interval: 36
sr-ease: 230
---
Logical Volume Manager - это слой абстракции над дисками в [[Linux]]
![[Excalidraw/LVM]]

Диск / Раздел:
- **`PV (Физический том)`**

|Команда|Что делает|
|---|---|
|`pvcreate /dev/sdb1`|Делает диск/раздел физическим томом LVM|
|`pvs`|Краткий список всех PV|
|`pvdisplay`|Подробная информация о PV|
|`pvremove /dev/sdb1`|Убирает LVM-метаданные с диска|
|`pvscan`|Сканирует систему на наличие PV|
- **`VG (Группа томов)`**

|Команда|Что делает|
|---|---|
|`vgcreate vg_name /dev/sdb1`|Создаёт VG из PV|
|`vgs`|Краткий список VG|
|`vgdisplay`|Подробная информация о VG|
|`vgextend vg_name /dev/sdc1`|Добавляет PV в VG|
|`vgreduce vg_name /dev/sdb1`|Убирает PV из VG|
|`vgremove vg_name`|Удаляет VG|
- **`LV (Логический том)`**

| Команда                                   | Что делает                        |
| ----------------------------------------- | --------------------------------- |
| `lvcreate -L 10G -n lv_name vg_name`      | Создаёт LV фиксированного размера |
| `lvcreate -l 100%FREE -n lv_name vg_name` | Использует всё свободное место VG |
| `lvs`                                     | Краткий список LV                 |
| `lvdisplay`                               | Подробная информация о LV         |
| `lvremove /dev/vg/lv`                     | Удаляет LV                        |
| `lvresize -L +5G /dev/vg/lv`              | Увеличивает LV                    |
| `lvresize -L 10G /dev/vg/lv`              | Устанавливает точный размер       |
| `lvresize -r ...`                         | Меняет LV и файловую систему      |
| `lvreduce -L 5G /dev/vg/lv`               | Уменьшает LV ⚠️ риск              |
- **`FS (ext4 / xfs / swap)`**


