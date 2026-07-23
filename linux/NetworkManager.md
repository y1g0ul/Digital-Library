---
created-dt: 2026-07-23 12:51
tags:
  - review
---
Служба в [[Linux]] для управления сетевыми подключениями. Чаще всего используется в настольных дистрибутивах (Ubuntu Desktop, Fedora и др.) и может управлять Ethernet, Wi-Fi, VPN и другими типами подключений.

``` bash
nmcli <команда>
```

| Команда | Что делает |
|---------|------------|
| `general status` | показать состояние NetworkManager |
| `device status` | показать сетевые интерфейсы |
| `connection show` | список сохранённых подключений |
| `connection up <имя>` | активировать подключение |
| `connection down <имя>` | отключить подключение |
| `connection delete <имя>` | удалить подключение |
| `device wifi list` | список доступных Wi-Fi сетей |
| `device connect <интерфейс>` | подключить интерфейс |

Основные службы:

| Служба | Назначение |
|--------|------------|
| `NetworkManager` | управляет сетевыми подключениями |
| `nmcli` | консольный интерфейс управления |
| `nmtui` | текстовый интерфейс управления |

Примеры:

``` bash
systemctl status NetworkManager      # проверить работу службы

nmcli general status                 # состояние NetworkManager
nmcli device status                  # список интерфейсов
nmcli connection show                # сохранённые подключения

nmcli connection up "Wired connection 1"     # включить подключение
nmcli connection down "Wired connection 1"   # отключить подключение

nmtui                                # текстовый интерфейс настройки сети
```

> **NetworkManager** обычно используется на настольных системах, а на серверах Ubuntu чаще применяется **`systemd-networkd`** через [[netplan]].

Проблема долгого ожиания при включении. Большинству домашних машин и виртуозок служба `wait-online`не нужна и ее можно выключить.
``` bash
sudo systemctl mask NetworkManager-wait-online.service
sudo systemctl mask systemd-networkd-wait-online.service
```

