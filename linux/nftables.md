---
created-dt: 2026-08-03 11:11
tags:
  - review
sr-due: 2026-09-10
sr-interval: 1
sr-ease: 130
---
Современная система [[Linux]] для управления межсетевым экраном (Firewall) ядра Linux. Является преемником [[iptables]] и используется для фильтрации, изменения и перенаправления сетевых пакетов.

``` bash
nft <команда> <объект> [условия] [действие]
```

``` 
Table
 ├── Chain
 │    ├── Rule
 │    ├── Rule
 │    └── Rule
 └── Chain
      └── Rule
```

Table - контейнер для цепочек и правил. Принадлежит определённому семейству сетевых протоколов.

|Семейство| Назначение    |
|---|---|
|`ip`| [[IPv4]]      |
|`ip6`| [[IPv6]]      |
|`inet`| IPv4 + IPv6   |
|`arp`| ARP           |
|`bridge`| Bridge-трафик |
``` bash
nft add table inet filter       # создать таблицу
nft list tables                 # показать таблицы
nft delete table inet filter    # удалить таблицу
```

Chain - цепочка правил, которые проверяются последовательно.

|Hook|Назначение|
|---|---|
|`input`|входящий трафик к компьютеру|
|`output`|исходящий трафик от компьютера|
|`forward`|трафик, проходящий через компьютер|
|`prerouting`|обработка до маршрутизации|
|`postrouting`|обработка после маршрутизации|
Пример цепочки Firewall:
``` bash
nft 'add chain inet filter input { type filter hook input priority 0; policy drop; }'
```

| Параметр      | Значение                      |
| ------------- | ----------------------------- |
| `type filter` | цепочка фильтрации            |
| `hook input`  | обрабатывает входящие пакеты  |
| `priority 0`  | приоритет обработки           |
| `policy drop` | запрещать пакеты по умолчанию |
Rule - отдельное правило, состоящее из условий и действия.
``` bash
nft add rule inet filter input tcp dport 22 accept
```

Основные условия

|Условие|Что проверяет|
|---|---|
|`ip saddr`|IPv4-адрес источника|
|`ip daddr`|IPv4-адрес назначения|
|`ip6 saddr`|IPv6-адрес источника|
|`tcp dport`|TCP-порт назначения|
|`tcp sport`|TCP-порт источника|
|`udp dport`|UDP-порт назначения|
|`iifname`|входящий интерфейс|
|`oifname`|исходящий интерфейс|
Основные действия

|Действие|Что делает|
|---|---|
|`accept`|разрешить пакет|
|`drop`|молча отбросить пакет|
|`reject`|отбросить пакет и отправить ответ|
|`dnat`|изменить адрес назначения|
|`snat`|изменить адрес источника|
|`masquerade`|SNAT с динамическим IP|
|`redirect`|перенаправить на локальный порт|

Управление правилами

| Команда   | Что делает                  |
| --------- | --------------------------- |
| `add`     | добавить объект или правило |
| `insert`  | вставить правило            |
| `delete`  | удалить объект или правило  |
| `replace` | заменить правило            |
| `list`    | показать объект или правила |
| `flush`   | очистить правила            |
 ``` bash
 nft list ruleset
# показать всю конфигурацию

nft list table inet filter
# показать таблицу

nft list chain inet filter input
# показать цепочку

nft -a list chain inet filter input
# показать правила вместе с handle

nft delete rule inet filter input handle 5
# Удаление правила по handle

nft flush chain inet filter input
# Очистить цепочку

nft flush table inet filter
# Очистить таблицу

sudo cp /etc/nftables.conf /etc/nftables.conf.bak 
sudo nft list ruleset > /etc/nftables.conf
# Сохранить старый конфиг и добавить новый
 ```


**`DNAT`**
Изменяет адрес назначения и используется для проброса портов.
``` 
SERVER:2222
     ↓
192.168.1.10:22
```


``` bash
nft add table ip nat

nft 'add chain ip nat prerouting { type nat hook prerouting priority -100; }'

nft add rule ip nat prerouting tcp dport 2222 dnat to 192.168.1.10:22
```

**`REDIRECT`**
Перенаправляет трафик на другой порт этого же компьютера.
``` 
localhost:80
      ↓
localhost:8080
```

``` bash
nft add rule ip nat prerouting tcp dport 80 redirect to :8080
```

**`MASQUERADE`**
Подменяет исходный [[networks/IP|IP]] динамическим адресом интерфейса. Часто используется для выхода локальной сети в интернет.
```
192.168.1.10
      ↓
Linux Server
      ↓
 Internet
```

``` bash
nft add rule ip nat postrouting oifname "eth0" masquerade
```

**`SNAT`**
Изменяет исходный IP на указанный адрес.
``` bash
nft add rule ip nat postrouting oifname "eth0" snat to 203.0.113.10
```

**`Базовый Firewall`**
``` bash
nft add table inet filter
# создать таблицу

nft 'add chain inet filter input { type filter hook input priority 0; policy drop; }'
# создать цепочку INPUT с политикой DROP

nft add rule inet filter input tcp dport 22 accept
# разрешить SSH

nft add rule inet filter input tcp dport 80 accept
# разрешить HTTP

nft add rule inet filter input tcp dport 443 accept
# разрешить HTTPS

nft add rule inet filter input iifname "lo" accept
# разрешить loopback

nft add rule inet filter input ct state established,related accept
# разрешить уже установленные соединения

nft add rule inet filter input ip saddr 192.168.1.100 drop
# заблокировать конкретный IP

nft add rule inet filter input ip saddr 192.168.1.100 tcp dport 22 drop
# заблокировать SSH от конкретного IP

nft add rule inet filter input ip saddr 192.168.1.0/24 tcp dport 22 accept
# разрешить SSH только из локальной сети

nft add rule inet filter input udp dport 51820 accept
# разрешить UDP-порт

nft add rule inet filter input ip protocol icmp accept
# разрешить ICMP
```

**`Работа с интерфейсами`**
``` bash
nft add rule inet filter input iifname "eth0" accept
# разрешить весь входящий трафик через eth0

nft add rule inet filter input iifname "eth0" tcp dport 22 accept
# разрешить SSH только через eth0

nft 'add chain inet filter output { type filter hook output priority 0; policy accept; }'
# создать цепочку OUTPUT с разрешением исходящего трафика
```

**`Проброс портов`**
``` bash
nft add table ip nat
# создать таблицу NAT

nft 'add chain ip nat prerouting { type nat hook prerouting priority -100; }'
# создать цепочку PREROUTING

nft add rule ip nat prerouting tcp dport 2222 dnat to 192.168.1.10:22
# пробросить порт 2222 на 192.168.1.10:22

nft add rule ip nat prerouting tcp dport 80 redirect to :8080
# перенаправить локальный порт 80 на 8080

nft 'add chain ip nat postrouting { type nat hook postrouting priority 100; }'
# создать цепочку POSTROUTING

nft add rule ip nat postrouting oifname "eth0" masquerade
# включить MASQUERADE при выходе через eth0

nft add rule ip nat postrouting oifname "eth0" snat to 203.0.113.10
# заменить исходный IP на указанный
```

**`Просмотр и управление`**
``` bash
nft list ruleset
# показать всю конфигурацию

nft list tables
# показать таблицы

nft list table inet filter
# показать таблицу

nft list chain inet filter input
# показать цепочку

nft -a list chain inet filter input
# показать правила вместе с handle

nft -a list ruleset
# показать все правила вместе с handle

nft delete rule inet filter input handle 5
# удалить правило по handle

nft flush chain inet filter input
# очистить цепочку

nft flush table inet filter
# очистить таблицу

nft monitor
# наблюдать изменения конфигурации
```