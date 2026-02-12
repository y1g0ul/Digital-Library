---
created-dt: 2026-01-13 07:03
tags:
  - review
sr-due: 2026-02-22
sr-interval: 10
sr-ease: 230
---
Инструмент в [[Linux]] для просмотра и отладки работы [[udev]].

**`udevadm info`**
- информация об устройстве
- атрибуты для udev rules
  Ключи:
	`--name=/dev/XXX` — указать устройство
	`--path=/sys/...` — указать путь в sysfs
	`--attribute-walk` — все атрибуты устройства и родителей
	`--query=property` — свойства устройства

**`udevadm monitor`**
- отслеживание событий устройств
- `add / remove / change`
  Ключи:
	`--kernel` — события ядра
	`--udev` — события udev
	`--property` — показывать свойства устройства

**`udevadm control`**
- управление демоном udev
- перезагрузка правил
  Ключи:
	`--reload-rules` — перезагрузить правила
	`--log-priority=debug` — включить отладку

**`udevadm trigger`**
- повторно отправить события
- применить правила к устройствам
  Ключи:
	`--action=add` — событие добавления
    `--subsystem-match=usb` — только USB
    `--type=devices` — реальные устройства

 **`udevadm settle`**
- ожидание завершения всех событий udev
  Ключи:
	`--timeout=SECONDS` — время ожидания

**`udevadm test`**
- проверка udev rules для устройства
- без реального выполнения действий
  Ключи:
	`--action=add` — тест события добавления

**`udevadm verify`**
- проверка синтаксиса udev rules
  Ключи:
	`--root=/etc/udev/rules.d` — путь к правилам

**`udevadm wait`**
- ожидание появления устройства
  Ключи:
	`--timeout=SECONDS` — ждать ограниченное время
