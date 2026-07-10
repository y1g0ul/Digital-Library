---
created-dt: 2026-06-09 14:43
tags:
  - review
sr-due: 2026-08-26
sr-interval: 47
sr-ease: 250
---
Команда в [[Linux]] для управления системным временем, часовым поясом и синхронизацией через NTP
``` bash
timedatectl [команда]
```

| Ключ             | Что делает                                       |
| ---------------- | ------------------------------------------------ |
| `status`         | показать текущее состояние времени               |
| `set-time`       | установить дату и время                          |
| `set-timezone`   | изменить часовой пояс                            |
| `list-timezones` | показать доступные часовые пояса                 |
| `set-ntp`        | включить или отключить NTP                       |
| `show`           | подробная информация в формате свойство=значение |
``` bash
timedatectl                    # показать текущее состояние

timedatectl status             # то же самое

timedatectl list-timezones     # список часовых поясов

timedatectl set-timezone Asia/Singapore
# установить часовой пояс

timedatectl set-timezone Europe/Moscow

sudo timedatectl set-time "2026-06-09 15:30:00"
# установить дату и время

sudo timedatectl set-ntp true
# включить синхронизацию времени

sudo timedatectl set-ntp false
# отключить синхронизацию времени

timedatectl show
# подробная информация
```

