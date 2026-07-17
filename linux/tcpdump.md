---
created-dt: 2026-07-17 13:20
tags:
  - review
---
Команда в [[Linux]] для захвата и анализа сетевых пакетов. Используется для диагностики сети, анализа трафика и поиска сетевых проблем.

```bash
tcpdump [опции] [фильтр]
```

|Ключ|Что делает|
|---|---|
|`-i`|Указать сетевой интерфейс|
|`-D`|Показать список доступных интерфейсов|
|`-n`|Не выполнять DNS-разрешение|
|`-nn`|Не разрешать DNS и номера портов|
|`-c`|Захватить указанное количество пакетов|
|`-w`|Сохранить пакеты в файл|
|`-r`|Прочитать пакеты из файла|
|`-A`|Показать содержимое пакета в ASCII|
|`-X`|Показать содержимое в HEX и ASCII|
|`-v`|Подробный вывод|
|`-vv`|Более подробный вывод|
|`-vvv`|Максимально подробный вывод|

**`Основные примеры`**

```bash
sudo tcpdump # Захватывать пакеты на основном интерфейсе
sudo tcpdump -i eth0 # Захватывать пакеты на интерфейсе eth0
sudo tcpdump -i any # Захватывать пакеты на всех интерфейсах
sudo tcpdump -D # Показать доступные интерфейсы
```

**`Фильтрация по хосту`**

```bash
sudo tcpdump host 192.168.1.10 # Трафик узла
sudo tcpdump src host 192.168.1.10 # Только исходящий от узла
sudo tcpdump dst host 192.168.1.10 # Только входящий к узлу
```

**`Фильтрация по порту`**

```bash
sudo tcpdump port 80 # Трафик порта 80
sudo tcpdump port 443 # HTTPS
sudo tcpdump src port 22 # Исходящий SSH
sudo tcpdump dst port 53 # DNS-запросы
```

**`Фильтрация по протоколу`**

```bash
sudo tcpdump tcp # Только TCP
sudo tcpdump udp # Только UDP
sudo tcpdump icmp # Только ICMP
sudo tcpdump arp # Только ARP
```

**`Логические операторы`**

```bash
sudo tcpdump 'tcp and port 80' # TCP на порту 80
sudo tcpdump 'host 192.168.1.10 and port 22' # SSH определенного узла
sudo tcpdump 'port 80 or port 443' # HTTP или HTTPS
sudo tcpdump 'not port 22' # Исключить SSH
```

**`Сохранение и чтение дампа`**

```bash
sudo tcpdump -w capture.pcap # Сохранить трафик в файл
sudo tcpdump -c 100 -w capture.pcap # Сохранить первые 100 пакетов
tcpdump -r capture.pcap # Прочитать дамп
```

**`Просмотр содержимого пакетов`**

```bash
sudo tcpdump -A port 80 # ASCII
sudo tcpdump -X port 80 # HEX + ASCII
sudo tcpdump -nn # Не выполнять DNS и разрешение портов
```

> [!info] Как работает tcpdump
> `tcpdump` захватывает пакеты непосредственно с сетевого интерфейса с помощью библиотеки **libpcap**. По умолчанию отображаются все пакеты, проходящие через выбранный интерфейс.
