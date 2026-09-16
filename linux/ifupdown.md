---
created-dt: 2026-09-14 11:56
tags:
  - review
sr-due: 2026-09-21
sr-interval: 5
sr-ease: 249
---
Классический набор утилит в [[Linux]] для настройки сетевых интерфейсов через файл:
```text
/etc/network/interfaces
```

В основном ассоциируется с Debian и старыми Ubuntu-системами.

Основные команды:
```bash
ifup <интерфейс>
# поднять интерфейс

ifdown <интерфейс>
# отключить интерфейс
```

Например:
```bash
sudo ifup eth0
# применить конфигурацию eth0

sudo ifdown eth0
# отключить eth0
```

### Конфигурация

Главный файл:
```text
/etc/network/interfaces
```

Пример DHCP:
```text
auto eth0
iface eth0 inet dhcp
```

Расшифровка:
```text
auto eth0
→ поднимать интерфейс автоматически при загрузке

iface eth0 inet dhcp
→ IPv4-конфигурация eth0 через DHCP
```

Пример статического IP:
```text
auto eth0

iface eth0 inet static
    address 192.168.1.10/24
    gateway 192.168.1.1
```

Дополнительные конфиги часто подключаются через:
```text
/etc/network/interfaces.d/
```

### Основные директивы

|Директива|Что значит|
|---|---|
|`auto`|поднимать интерфейс при загрузке|
|`allow-hotplug`|поднимать интерфейс при его появлении|
|`iface`|начало конфигурации интерфейса|
|`inet`|IPv4|
|`inet6`|IPv6|
|`dhcp`|получить адрес через DHCP|
|`static`|статическая конфигурация|
|`manual`|не назначать IP автоматически|

Пример:
```text
allow-hotplug enp1s0

iface enp1s0 inet dhcp
```

### Как это работает

Упрощённо:
```text
/etc/network/interfaces
        ↓
ifupdown
        ↓
ifup / ifdown
        ↓
настройка интерфейса, IP и маршрутов
```

Сам `ifupdown` не является сетевым демоном вроде [[NetworkManager]].

Это скорее система:
```text
конфигурационный файл
+
утилиты для применения конфигурации
```

### В современных системах

Сегодня часто используются другие системы управления сетью:
```text
Desktop
→ NetworkManager

Ubuntu Server
→ Netplan
   ↓
   systemd-networkd / NetworkManager

многие серверы
→ systemd-networkd
```

`ifupdown` всё ещё встречается прежде всего в Debian-системах и старых конфигурациях.

Важно не смешивать несколько сетевых менеджеров для одного интерфейса:
```text
ifupdown
NetworkManager
systemd-networkd
```

иначе они могут пытаться управлять одним интерфейсом одновременно.

### Главное отличие

```text
ifupdown
→ /etc/network/interfaces

NetworkManager
→ connections + nmcli

systemd-networkd
→ /etc/systemd/network/*.network

Netplan
→ YAML-конфигурация
→ генерирует конфиг для backend
```