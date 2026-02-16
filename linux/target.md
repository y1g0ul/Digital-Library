---
created-dt: 2026-01-15 07:29
tags:
  - review
sr-due: 2026-02-13
sr-interval: 18
sr-ease: 250
---
это [[unit]]-группа, которая:
- объединяет другие [[unit]] (services, sockets, mounts и т.д.)
- описывает **состояние системы**
- управляет тем, какие части системы должны быть запущены
``` shell
graphical.target
 ├─ network.service
 ├─ ssh.service
 ├─ docker.service
 └─ display-manager.service

```

Таргеты могут быть активны однавременно:
`graphical.target`
- включает `multi-user.target`
    - включает `basic.target`
        - включает `sysinit.target`


Основные таргеты:

**`sysinit.target`**
- самая ранняя инициализация
- монтирование `/`, [[swap]], [[udev]]
- база всей системы

**`basic.target`**
- базовые службы [[systemd]]
- foundation для всего остального

**`multi-user.target`**
- полноценная система **без GUI**
- сеть
- демоны
- SSH
- аналог runlevel 3

**`graphical.target`**
- `multi-user.target` + графика
- дисплейный менеджер
- аналог runlevel 5

**`rescue.target`**
- однопользовательский режим
- root shell
- минимум сервисов
- используется для восстановления

**`emergency.target`**
- ещё жестче чем rescue
- даже `/` может быть read-only
- используется при серьёзных поломках

**`poweroff.target`**
- выключение системы

**`reboot.target`**
- перезагрузка системы

Основные команды

| Команда                                        | Что делает                                                                                            |
| ---------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| `systemctl list-units --type=target`           | Показывает **активные** таргеты                                                                       |
| `systemctl list-unit-files --type=target`      | Показывает все **установленные** таргеты**                                                            |
| `systemctl get-default`                        | Показывает таргеты **по умолчанию**                                                                   |
| `sudo systemctl set-default graphical.target`  | Устанавливает GUI по умолчанию                                                                        |
| `sudo systemctl set-default multi-user.target` | Устанавливает серверный режим                                                                         |
| `sudo systemctl isolate name.target`           | Переключает систему в указанный таргет. Останавливает все, что не является зависимостью этого таргета |
| `systemctl list-dependencies name.target`      | Показывает зависимости таргета                                                                        |
| `systemctl is-active name.target`              | Проверяет, активен ли таргет                                                                          |
| `systemctl is-enabled name.target`             | Проверяет, включён ли таргет                                                                          |
| `systemctl status name.target`                 | Подробный статус таргета                                                                              |
