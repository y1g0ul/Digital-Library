---
created-dt: 2026-06-11 10:26
tags:
  - review
---
Команда в [[Linux]] для работы с аппаратными часами (RTC)
``` bash
hwclock [опции]
```

| Ключ           | Что делает                                        |
| -------------- | ------------------------------------------------- |
| `--show`, `-r` | показать время RTC                                |
| `--set`        | установить время RTC                              |
| `--hctosys`    | скопировать время из RTC в системные часы         |
| `--systohc`    | скопировать системное время в RTC                 |
| `--utc`        | RTC хранит время в UTC                            |
| `--localtime`  | RTC хранит локальное время                        |
| `--adjust`     | скорректировать время по данным из `/etc/adjtime` |
| `--test`       | показать действия без выполнения                  |
| `--verbose`    | подробный вывод                                   |
``` bash
hwclock --show                # показать время RTC

hwclock -r                    # то же самое

sudo hwclock --hctosys        # RTC -> системные часы

sudo hwclock --systohc        # система -> RTC

sudo hwclock --utc --systohc  # записать UTC в RTC

sudo hwclock --localtime --systohc
# записать локальное время в RTC

sudo hwclock --set --date "2026-06-11 12:00:00"
# установить время RTC

hwclock --verbose             # подробная информация
```

