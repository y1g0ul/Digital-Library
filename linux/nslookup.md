---
created-dt: 2026-09-28 16:44
tags:
  - review
---
Команда в [[Linux]] для выполнения DNS-запросов.

Позволяет узнать, в какой IP-адрес преобразуется доменное имя, а также запросить отдельные типы DNS-записей.

```
nslookup [домен] [DNS-сервер]
```

### Примеры

```
nslookup google.com
# узнать IP-адрес домена

nslookup google.com 8.8.8.8
# выполнить запрос через конкретный DNS-сервер

nslookup 8.8.8.8
# обратный DNS-запрос: попытаться узнать имя по IP
```

Пример вывода:

```
Server:         192.168.1.1
Address:        192.168.1.1#53

Non-authoritative answer:
Name:   google.com
Address: 142.250.74.14
```

Где:

```
Server
→ DNS-сервер, который обработал запрос

Address: ...#53
→ адрес DNS-сервера и порт 53

Name / Address
→ результат DNS-запроса
```

### Типы DNS-записей

Через `-type` можно запросить конкретную запись.

|Тип|Что содержит|
|---|---|
|`A`|IPv4-адрес|
|`AAAA`|IPv6-адрес|
|`MX`|почтовые серверы|
|`NS`|DNS-серверы домена|
|`TXT`|текстовые записи|
|`CNAME`|псевдоним домена|
|`SOA`|основная информация о DNS-зоне|

Примеры:

```
nslookup -type=A example.com
# IPv4

nslookup -type=AAAA example.com
# IPv6

nslookup -type=MX example.com
# почтовые серверы

nslookup -type=NS example.com
# authoritative DNS-серверы

nslookup -type=TXT example.com
# TXT-записи
```

### Интерактивный режим

Если запустить без аргументов:

```
nslookup
```

откроется интерактивный режим:

```
> server 8.8.8.8
> set type=MX
> example.com
> exit
```

### nslookup и dig

```
nslookup
→ проще и быстрее для базовых DNS-проверок

dig
→ больше подробностей и возможностей для диагностики DNS
```

Для серьёзного DNS-траблшутинга чаще используют [[dig]], а `nslookup` удобен для быстрых проверок.

Главная идея:

```
домен
  ↓
nslookup
  ↓
DNS-сервер
  ↓
IP / MX / NS / TXT / другие записи
```