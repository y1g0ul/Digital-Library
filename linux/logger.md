---
created-dt: 2026-06-16 10:28
tags:
  - review
sr-due: 2026-07-24
sr-interval: 18
sr-ease: 228
---
Команда в [[Linux]] для отправки сообщений в системный журнал ([[rsyslog]]/[[journalctl]])

``` bash
logger [опции] сообщение
```

| Ключ         | Что делает                                     |
| ------------ | ---------------------------------------------- |
| `-p`         | указать facility и priority                    |
| `-t`         | задать тег сообщения                           |
| `-i`         | добавить PID процесса                          |
| `-f`         | прочитать сообщение из файла                   |
| `-s`         | вывести сообщение также в stderr               |
| `--journald` | отправить сообщение напрямую в journald        |
| `--id`       | использовать указанный PID                     |
| `--server`   | отправить сообщение на удалённый syslog-сервер |
| `--udp`      | использовать UDP                               |
| `--tcp`      | использовать TCP                               |
``` bash
logger "Hello World"

logger "Backup completed"

logger -t backup "Backup completed"

logger -i "Service restarted"

logger -p user.warning "Disk usage is high"

logger -p local0.info "Application started"

logger -f message.txt

echo "Test" | logger

logger --server 192.168.1.10 --udp \
"Test message"
```
