---
created-dt: 2026-09-28 16:58
tags:
  - review
sr-due: 2026-10-16
sr-interval: 6
sr-ease: 234
---
Команда в [[Linux]] для просмотра и изменения параметров сетевого интерфейса на уровне драйвера и физического линка.

Особенно полезна для Ethernet-интерфейсов.

```
ethtool <интерфейс>
```

Например:
```
ethtool enp3s0
# показать состояние линка и его параметры
```

Обычно вывод содержит:
```
Speed: 1000Mb/s
Duplex: Full
Auto-negotiation: on
Link detected: yes
```

Расшифровка:

|Поле|Что значит|
|---|---|
|`Speed`|скорость линка|
|`Duplex`|режим передачи: Full/Half|
|`Auto-negotiation`|автоматическое согласование параметров|
|`Link detected`|есть ли физическое соединение|

### Полезные ключи

|Ключ|Что делает|
|---|---|
|`-i`|информация о драйвере|
|`-S`|статистика интерфейса|
|`-k`|показать offload-функции|
|`-K`|изменить offload-функции|
|`-g`|параметры RX/TX ring buffer|
|`-G`|изменить ring buffer|
|`-c`|настройки interrupt coalescing|
|`-C`|изменить interrupt coalescing|
|`-a`|настройки pause frames|
|`-s`|изменить speed/duplex/autoneg|
|`-p`|заставить индикатор порта мигать|

### Драйвер

```
ethtool -i enp3s0
# показать драйвер сетевой карты
```

Пример:
```
driver: r8169
version: ...
firmware-version: ...
bus-info: 0000:03:00.0
```

### Статистика

```
ethtool -S enp3s0
# показать статистику интерфейса
```

Можно увидеть:
```
rx_packets
tx_packets
rx_errors
tx_errors
rx_dropped
tx_dropped
```

Полезно при поиске проблем с сетью.

### Offload

Сетевая карта может брать часть работы CPU на себя.

```
ethtool -k enp3s0
# показать offload-функции
```

Например:
```
rx-checksumming
tx-checksumming
tcp-segmentation-offload
generic-segmentation-offload
```

Упрощённо:
```
без offload
CPU делает больше сетевой обработки

с offload
часть работы выполняет NIC
```

Изменить:
```
sudo ethtool -K enp3s0 tso off
# отключить TCP Segmentation Offload
```

Обычно offload не меняют без причины.

### Скорость и duplex

```
sudo ethtool -s enp3s0 speed 1000 duplex full autoneg on
# задать параметры линка
```

В обычной ситуации лучше оставлять:
```
Auto-negotiation: on
```

чтобы интерфейс сам согласовал скорость и duplex с коммутатором.

### Ring buffer

```
ethtool -g enp3s0
# посмотреть размеры
```