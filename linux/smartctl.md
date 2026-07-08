---
created-dt: 2026-07-08 11:38
tags:
  - review
---
Команда в [[Linux]] для просмотра S.M.A.R.T. информации, диагностики и тестирования HDD, SSD и NVMe-накопителей
``` bash
smartctl [опции] устройство
```

| Ключ     | Что делает                             |
| -------- | -------------------------------------- |
| `-a`     | показать всю доступную информацию      |
| `-i`     | информация об устройстве               |
| `-H`     | показать общее состояние SMART         |
| `-A`     | таблица SMART-атрибутов                |
| `-x`     | максимально подробная информация       |
| `-c`     | показать возможности и настройки SMART |
| `-l`     | просмотреть журнал (лог)               |
| `-t`     | запустить самотестирование             |
| `-X`     | прервать выполняющийся тест            |
| `-s on`  | включить SMART                         |
| `-s off` | выключить SMART                        |

``` bash
sudo smartctl -t short /dev/sda # Короткий тест
sudo smartctl -t long /dev/sda # Расширенный тест
sudo smartctl -t convey /dev/sda # Тест транспортного интерфейса
sudo smartctl -l selftest /dev/sda # Показать результаты тестов
sudo smartctl -X /dev/sda # Прервать тест
sudo smartctl -l error /dev/sda # Журнал ошибок
sudo smartctl -H /dev/sda # Проверка состояния
```

**`Основные SMART-атрибуты`**

|Атрибут|Описание|
|---|---|
|`Reallocated_Sector_Ct`|переназначенные сектора|
|`Current_Pending_Sector`|нестабильные сектора|
|`Offline_Uncorrectable`|неисправимые ошибки чтения|
|`Power_On_Hours`|время работы диска|
|`Power_Cycle_Count`|количество включений|
|`Temperature_Celsius`|температура|
|`Wear_Leveling_Count`|износ SSD|
|`Media_Wearout_Indicator`|остаточный ресурс SSD|
Служба называется **smartd**. 