---
created-dt: 2026-09-28 17:17
tags:
  - review
sr-due: 2026-10-19
sr-interval: 9
sr-ease: 263
---
Команда в [[Linux]] для просмотра и управления [[DNS]]-настройками через [[systemd]].

Позволяет посмотреть:
- какие DNS-серверы используются;
- DNS для конкретного интерфейса;
- поисковые домены;
- статус DNS;
- выполнить DNS-запрос.

```
resolvectl [команда] [параметры]
```

### Основные команды

|Команда|Что делает|
|---|---|
|`status`|показать текущие DNS-настройки|
|`query`|выполнить DNS-запрос|
|`dns`|посмотреть или задать DNS для интерфейса|
|`domain`|посмотреть или задать search domain|
|`default-route`|управлять использованием интерфейса как DNS default route|
|`flush-caches`|очистить DNS-кеш|
|`statistics`|показать статистику DNS-кеша|
|`reset-statistics`|сбросить статистику|

### Просмотр DNS

```
resolvectl status
# показать общую DNS-конфигурацию

resolvectl status enp3s0
# DNS-настройки конкретного интерфейса

resolvectl dns
# показать DNS-серверы интерфейсов

resolvectl domain
# показать DNS-домены
```

Пример:
```
Link 2 (enp3s0)
    Current Scopes: DNS
         DNS Servers: 192.168.1.1
                      1.1.1.1
          DNS Domain: ~.
```

Где:
```
DNS Servers
→ DNS-серверы интерфейса

DNS Domain
→ домены, для которых используется этот интерфейс

~.
→ этот интерфейс используется как маршрут для всех DNS-запросов
```

### DNS-запрос
```
resolvectl query google.com
# узнать адрес домена

resolvectl query 8.8.8.8
# обратный DNS-запрос
```

Пример:
```
google.com: 142.250.74.14
            2a00:1450:4001:...
```

### Изменение DNS

```
sudo resolvectl dns enp3s0 1.1.1.1 8.8.8.8
# задать DNS для интерфейса

sudo resolvectl domain enp3s0 example.com
# задать search domain
```

Обычно такие изменения временные и действуют до перезапуска интерфейса или изменения конфигурации сетевым менеджером.

Для постоянной настройки DNS обычно используют:
```
NetworkManager
systemd-networkd
Netplan
```

### Очистка DNS-кеша

```
sudo resolvectl flush-caches
# очистить DNS-кеш
```

Посмотреть статистику:
```
resolvectl statistics
# показать cache hits, misses и другую статистику
```

### Как это связано с systemd-resolved

Упрощённо:
```
приложение
   ↓
systemd-resolved
   ↓
DNS-сервер интерфейса
   ↓
DNS
```

`resolvectl` - это утилита для управления и диагностики.

### resolvectl и другие DNS-команды

```
resolvectl
→ DNS-настройки самой Linux-системы

dig
→ подробная диагностика DNS-запросов

nslookup
→ простой DNS-запрос
```

Главная идея:
```
resolvectl
→ посмотреть, какой DNS использует система
→ проверить DNS по интерфейсам
→ выполнить запрос
→ очистить DNS-кеш
```