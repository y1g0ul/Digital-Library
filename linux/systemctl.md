---
created-dt: 2026-01-14 09:23
tags:
  - review
sr-due: 2026-04-04
sr-interval: 47
sr-ease: 250
---
Это основная команда в [[Linux]] для управления всеми [[unit]] [[systemd]].


Основные команды для сервисов

| Команда   | Что делает                               | Пример                          |
| --------- | ---------------------------------------- | ------------------------------- |
| `start`   | Запустить сервис                         | `sudo systemctl start ssh`      |
| `stop`    | Остановить                               | `sudo systemctl stop nginx`     |
| `restart` | Перезапустить                            | `sudo systemctl restart docker` |
| `reload`  | Перезагрузить конфигурацию без остановки | `sudo systemctl reload ssh`     |
| `status`  | Посмотреть состояние                     | `systemctl status ssh`          |
| `enable`  | Включить автозапуск                      | `sudo systemctl enable ssh`     |
| `disable` | Выключить автозапуск                     | `sudo systemctl disable ssh`    |

Работа с сокетами

| Команда  | Что делает      | Пример                            |
| -------- | --------------- | --------------------------------- |
| `start`  | Запустить сокет | `sudo systemctl start ssh.socket` |
| `status` | Проверить порт  | `systemctl status ssh.socket`     |
Работа с всеми [[unit]]

| Команда           | Что делает                                         |
| ----------------- | -------------------------------------------------- |
| `list-units`      | Все активные юниты                                 |
| `list-unit-files` | Все [[unit]] файлы (активные и нет)                |
| `daemon-reexec`   | Перезапуск [[systemd]] (чтобы увидеть новые файлы) |
| `cat <unit>`      | Показать итоговую конфигурацию [[unit]]            |
Часто используемые ключи

| Ключ         | Что делает                                               | Пример                                 |
| ------------ | -------------------------------------------------------- | -------------------------------------- |
| `--type=`    | Фильтр по типу [[unit]] (service, socket, mount, device) | `systemctl list-units --type=service`  |
| `--all`      | Показывает все [[unit]], даже неактивные                 | `systemctl list-units --all`           |
| `--failed`   | Показать только неудачные/упавшие [[unit]]               | `systemctl --failed`                   |
| `--state=`   | Фильтр по состоянию (active, inactive, failed, etc.)     | `systemctl list-units --state=failed`  |
| `--no-pager` | Не использовать постраничный просмотр                    | `systemctl status ssh --no-pager`      |
| `--user`     | Действия для юзерских [[unit]]                           | `systemctl --user list-units`          |
| `--system`   | Действия для системных [[unit]] (по умолчанию)           | `systemctl --system status ssh`        |
| `--quiet`    | Показывает минимальный вывод                             | `systemctl list-units --quiet`         |
| `--lines=N`  | Сколько строк журнала выводить                           | `journalctl -u ssh.service --lines=20` |
